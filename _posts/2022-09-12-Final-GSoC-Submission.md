---
layout: post
title: "My GSoC Journey - Part 5"
subtitle: "Final Submission !"
date: 2022-07-20
background: '/img/posts/gsoc-final/finish-line.webp'
tags: gsoc
---

*After an adventurous journey throughout these five months, starting with knowing very little about GNU, Python packaging, Docker, Build systems and CI/CD to having a Python package on PyPI, this is a final summary of all my work throughout GSoC 2022!*

## About pyGnuastro

### Links

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Topic<br></th>
    <th class="tg-0pky">Links</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">GitHub</td>
    <td class="tg-0pky"><a href="https://github.com/Jash-Shah/pyGnuastro"><span style="color:#905">https://github.com/Jash-Shah/pyGnuastro</span></a></td>
  </tr>
  <tr>
    <td class="tg-0pky">PyPI</td>
    <td class="tg-0pky"><a href="https://test.pypi.org/project/pygnuastro/"><span style="color:#905">https://test.pypi.org/project/pygnuastro/</span></a></td>
  </tr>
  <tr>
    <td class="tg-0pky">Documentation</td>
    <td class="tg-0pky">&lt;ADD LINK TO DOCS&gt;</td>
  </tr>
  <tr>
    <td class="tg-0pky">Docker Images</td>
    <td class="tg-0pky"><a href="https://hub.docker.com/repository/registry-1.docker.io/jashshah1401/pygnuastro/tags?page=1&amp;ordering=last_updated"><span style="color:#905">https://hub.docker.com/repository/registry-1.docker.io/jashshah1401/pygnuastro/tags?page=1&amp;ordering=last_updated</span></a></td>
  </tr>
</tbody>
</table>
<br>

### Description
The pyGnuastro python package is a free software which aims to offer a wide array of functions
for astronomical data manipulation and analysis. It is the Python 
implementation of [GNU Astronomy Utilities](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/gnuastro.html)
and aims to offer all the functionality of Gnuastro
but with a higher level of abstracion.

### Need
While [Gnuastro's Library](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/Gnuastro-library.html)
and its [Binary programs](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/General-program-usage-tutorial.html)
provides extensive and fine-tuned functionality,
it's tough to deny the versitality and ease of usage that a language like
Python brings. Being dynamically typed and high-level makes
it simple for novice programmers to interact with Gnuastro's programs.
Additionally, since many of Gnuastro's use cases require working with data
arrays, integrating libraries like [NumPy](https://numpy.org/doc/stable/index.html)
makes data analysis and manipulation more accessible.





## Work done throughout GSoC

1. Learned about [Python Extensions](https://docs.python.org/3/extending/extending.html), [NumPy C-API](py.org/doc/stable/reference/c-api/index.html), [setuptools](https://setuptools.pypa.io/en/latest/userguide/ext_modules.html), [Gnuastro library](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/Library.html) and tried out some [examples](https://github.com/Jash-Shah/python-extension-practice) of the same.
  
2. Learned about the GNU Build system, mainly [Autoconf](https://www.gnu.org/software/autoconf/) and [Automake](https://www.gnu.org/software/automake/).
   
3. Started by trying to package the python modules with the Gnuastro itself. The first module tried was [`cosmology`](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/Cosmology-library.html) since it doesn't have any external dependencies and only a few basic functions. [Commit](https://github.com/Jash-Shah/gnuastro-jash/commit/6997730fab6cb18fd7b34f77f9a0f65f0c7e0730).

4. However, after discussing with my Mentor we came to the conclusion that trying to combine Python and GNU build systems wasn't the best approach. So we decided to make a separate Python package called `pyGnuastro` which would be distributed as its own separate Python package and be available on PyPI!

5. Added a [Python Interface](https://github.com/Jash-Shah/gnuastro-jash/commit/ed301e20c831a276da77dd1b8f138695f672359e)([doc (section 12.3.29 pg717) ](http://akhlaghi.org/gnuastro.pdf)) module to the Gnuastro Library to provide the utility functions that will be required by the Python wrappers.

6. Learned about [Python Wheels](https://realpython.com/python-wheels/). pyGnuastro depends on the Gnuastro library which in turn has its own [dependencies](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/Dependencies.html). So, in order to ensure a smooth installation experience for users, we make use of [`auditwheel`](https://pypi.org/pypi/auditwheel/)([`delocate`](https://pypi.org/project/delocate/) for MacOS) to build [`manylinux` wheels](https://github.com/pypa/manylinux). These wheels include all the dependencies in the distributed package itself rather than depending on the users to install, build and link to them :smile:

7. Created the [structure of the Python package](https://packaging.python.org/en/latest/overview/) with the [`setup.py`](https://github.com/Jash-Shah/pyGnuastro/blob/main/setup.py) script to build the extensions.

7. Built a [custom docker image](https://hub.docker.com/repository/registry-1.docker.io/jashshah1401/pygnuastro/tags?page=1&ordering=last_updated) using [`maneage`](http://www.maneage.org). Worked with my mentor to create a build script for maneage which builds the gnuastro library statically([repo with reproducible instructions](https://gitlab.com/makhlaghi/gnuastro-in-maneage-static)) by taking  `quay.io/pypa/manylinux2014_x86_64` as the base image. This ensures that only **one** library is included in the pyGnuastro distribution. This reduces the size, increases portability and speeds up the installation of pyGnuastro :stars:

8. Created a [workflow script](https://github.com/Jash-Shah/pyGnuastro/blob/main/.github/workflows/build.yml) using GitHub workflows to build wheels for `manylinux` images, MacOS and source distributions of pyGnuastro.





## Future Improvements
pyGnuastro is still in its ideation phase. The major work of figuring out the build system, setting up the docker image and the workflow now allows for a quick process to add more funcionality. The major improvements include:
- More concise documentation with focus on reproducibility
- Add more modules, currently we only have two i.e. `cosmology` and `fits`
- Add support for custom data types in Python for better interfacing. For eg: All the [list data types in Gnuastro](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/Linked-lists.html) can have their corresponding python versions
- Add support for more platforms. Create a system similar to the current implementation of the `maneage-manylinux2014_x86_64` docker for other manylinux images as well
- Add integration with other packages like OpenCV, matplotlib, scipy, etc just like NumPy.
- Better infrastructure and full automated maintainence of the GitHub repo to make it the sole place for all information related to pyGnuastro
- Add modules for the binary programs for Gnuastro as well just like the library functions. This can be done by either defining library functions which perform similar tasks as the binary program or implement some custom modules to replicate the behaviour of binary programs in Python
- Perform efficiency comparisons with other implementations once the package reaches a mature state





## Acknowledgements
I had a great time working with everyone at Gnuastro. I would like to specially thank my mentor, [Mohammad Akhlaghi](https://akhlaghi.org/) and my "unofficial" co-mentor [Pedram Ashofteh](https://pedramardakani.wordpress.com/) for their continuous support and motivation. The kind of community Mohammad has created with Gnuastro still amazes me. It felt more like being a part of a team than an organization at Gnuastro because of how inclusive, supportive, and collaborative the working environment is. I was always given tasks that were quite clear. All my questions were patiently and clearly addressed, no matter how "silly" they were. There were never any hard-set deadlines imposed upon me, which allowed me to explore the domains involved in my project more freely and made for an overall better experience. I also appreciate the fact that mistakes made by me were returned with concise criticism rather than being reprimanded for them. Through the mentorship, I learned both the technicalities and the philosophies behind writing good code. Even though I've not been able to meet all the deliverables mentioned in my initial proposal, I plan to work with Gnuastro to finish my project and also contribute to its codebase in general.