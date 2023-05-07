---
layout: post
title: "My GSoC Journey - Part 4"
subtitle: "Week 3 & 4 - How low can you go..."
date: 2022-07-20
background: '/img/posts/gsoc-week3_4/gnu_python.jpg'
tags: gsoc
---

*Writing the extension modules and Python wrappers for a package is one thing, but a step that is often overlooked is making a build system that complies with the rest of your program, ensures the correct installation based on your dependencies and also is portable enough to be distributable.*

*I learned these things the hard way in Week 3 and 4, where I went as low level as I could to try to solve all the weird build errors and glitches I had while **trying to build a Python Package using GNU Autotools**.*

## Building
As discussed in my last GSoC blog, I was mainly using `distutils` along with it's `distutils.setup` script to take care of all the building and linking required for building the `.so` (shared object) file required by the  Python Interpreter. However, one of my co-mentors brought up a good point that `setuptools` is the packaging tool that is recommended by [PyPA](https://packaging.python.org/en/latest/guides/tool-recommendations/) and also using `wheels` to package the modules instead of the standard `setup.py build` command.

Hence, **Week 3 was spent mainly learning about `setuptools` and `wheels`**. [What Are Python Wheels and Why Should You Care?](https://realpython.com/python-wheels/#the-manylinux-wheel-tag) is a great article to start with Python Wheels. The [setuptools documentation](https://setuptools.pypa.io/en/latest/index.html) is a great place to know about setuptools, if you already know about distutils like me! Luckily, while `Setuptools` is a "beefier" version of distutils, as it offers better and more packaging utilities, it keeps the same functions, so in terms of code it was just a change of one line for me.

Originally, with `distutils`, the plan was to have the files related to the Python Package in a separate *`python/`* directory at the root of the Gnuastro source like:
```
📦python
 ┣ 📂gnuastro.arithmetic
 ┃ ┣ 📜arithmetic.c
 ┃ ┗ 🔧setup.py
 ┣ 📂gnuastro.cosmology
 ┃ ┣ 📜cosmology.c
 ┃ ┗ 🔧setup.py
 ┣ 📂gnuastro.fits
 ┃ ┣ 📜fits.c
 ┃ ┗ 🔧setup.py
 ┗ 📑Makefile.am
 ```

 The idea was to have the `setup.py` script in each folder build that specific extension, and let the Makefile handle the linking. But I soon realized that this was too excessive. A better structure would be:
 ```
 📦python
 ┣ 📂src
 ┃ ┣ 📜arithmetic.c
 ┃ ┣ 📜cosmology.c
 ┃ ┗ 📜fits.c
 ┣ 📑Makefile.am
 ┗ 🔧setup.py
 ```

### Using Autotools to build Python Package
As the name suggests **GNU**astro is a GNU project, and thus depends on Autotools([Automake](https://www.gnu.org/software/automake/manual/html_node/index.html#SEC_Contents) and [Autoconf](https://www.gnu.org/software/autoconf/) and [Libtool](https://www.gnu.org/software/libtool/)) for its building and compiling. These are the tools behind the 
```
./configure
make 
make check
make install
```
set of instructions.

Alongwith the setup script, I also added a new file([`python.c`](https://github.com/Jash-Shah/gnuastro-jash/blob/6997730fab6cb18fd7b34f77f9a0f65f0c7e0730/lib/python.c)) to the *lib/* directory of Gnuastro. This file basically provides any utility functions I might require while building the Python package. Currently, the file provides type conversion functions, which facilitate converting between Gnuastro and NumPy's datatypes.

So, what is the difference between your traditional Makefile and using Autotools instead:-
- **Autoconf** easily scans an existing tree to find its dependencies and creates a configure script that will run under almost any kind of shell. The configure script allows the user to control the build behavior (i.e. --with-foo, --without-python, --prefix, --sysconfdir, etc..) as well as doing checks to ensure that the system can compile the program.

Configure generates a `config.h` file (from a template) which programs can include to work around portability issues. For example, if HAVE_NUMPY is not defined, don't build the Python package.

- **Automake** provides a short template that describes what programs will be built and what objects need to be linked to build them, thus Makefiles that adhere to GNU coding standards can automatically be created.
  
My job was to use these tools to also call the `setup` script for building my Python package. 


My approach to building the package using Autotools involved 4 basic steps:
1. Adding the necessary checks in the `configure.ac` script.
      - Check if a user has Python 3 on their system and get it's include path i.e. path to `Python.h` file.
      - If Python 3 is found, Check if the user has NumPy on their system and get it's include path.
      - Substitute the include paths as variables to be passed to all `Makefile.am's`.
2. Conditionally build the Python package and its utility functions module(`lib/python.c`) only if the above checks are passed.
3. Write the `Makefile.am` in the `python/` directory which would handle the *build, install, uninstall and clean* targets for the Python package.
4. Re-write the setup.py script to make it more generic, by using the environment variables passed by the configure script instead of hardcoding the include and install paths.
     - This also ensures that the Python package building supports [VPATH](https://www.gnu.org/software/automake/manual/html_node/VPATH-Builds.html) builds, which is another great feature of Autotools. For the uninitiated, `VPATH builds` are basically a way to separate your source and build tree, so that all the built files (.o, .so, etc) are in a separate directory than your source files but are symlinked to the source tree.

This process took a lot of trial and error, digging into the Autotools(mostly Automake) documentation and playing around with the `Makefile.am` to get right. But it introduced me to these amazing tools and taught me how to make any scrawny personal project distributable!

## Installing
After running,
```
python3 setup.py build_ext bdist_wheel
```
the distributable wheel file, with all of the package's metadata, is created under the `dist/` folder. In order to install this file we use pip as follows:
```
pip install Gnuastro.whl
```
YES! It is in fact as simple as that!

But there is an issue that I faced here, suppose that a user wants to install the Gnuastro library in their root directory, or to any directory where they dont have privileges. This means they'll run `sudo make install` from the root of the source. This cascades to calling the Makefile in the *python/* directory with root access as well. However, running `pip` with sudo access is a big NO, NO. And `pip` would warn you of that with a warning like:

<img src="{{site.baseurl}}/img/posts/gsoc-week3_4/pip_error.png" alt="PIP sudo error message" width="1000" height="50">

<!-- ![PIP sudo error message]({{site.baseurl}}/img/posts/gsoc-week3_4/pip_error.png) -->

This is because, Python packages are generally installed at a local level, in the `/usr/local` directory. However, if you call `pip` with `sudo` then it installs the packages in the root directory. To sove this, we use
```
sudo -u "$SUDO_USER pip install Gnuastro,whl
```
which basically runs the pip command as the user who called sudo. This will ensure that your package gets installed in the local directory instead of root!
