---
layout: post
title: "My GSoC Journey - Part 3"
subtitle: "Week 1 & 2 - Coding Begins"
date: 2022-07-01
background: '/img/posts/gsoc-coding-begins/bg-coding-begins.png'
tags: gsoc
---

## Coding Begins!
So, now that I got to know the Gnuastro community a bit and had discussed the plan of attack with my mentor it was time to start with the actual coding.

### Week 1
As planned, I started with the building the extension module for [Cosmic Calculator](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/CosmicCalculator.html)(cosmiccal) library. A simple Python extension Module should be structed as:

![Code-Block1]({{site.baseurl}}/img/posts/gsoc-coding-begins/code-block-1.png)

The cosmical library was chosen as a starting point because it contained only 6 functions and solely dealt with *doubles, ints, and floats*. Consequently, there wasn't yet a requirement for a NumPy Converter. It was pretty straight forward to create wrappers for these functions by following the aforementioned structure. The `setup.py` script for building and installing these modules was created at the following stage. For this, I followed the [Python Extension documentation's](https://docs.python.org/3/extending/building.html#building) advice and utilised [`distutils`](https://docs.python.org/3/distutils/apiref.html), which offers two crucial functions:
- [<u>distutils.core.Extension</u>](https://docs.python.org/3/distutils/apiref.html#distutils.core.Extension) which is used to describe a `C/C++` extension.
- [<u>distutils.core.setup</u>](https://docs.python.org/3/distutils/apiref.html#distutils.core.setup) the frontman in actually building and compiling the modules.

![Code-Block2]({{site.baseurl}}/img/posts/gsoc-coding-begins/code-block-2.png)

After this, the commands to build and install these modules were simply:
```bash
python3 setup.py build
python3 setup.py install
```

### Week 2
At our subsequent meeting, my mentor confirmed my work, and we both agreed that the next step should be to write the NumPy converter so that this may be *expanded* to include the other library modules as well.

Week 2 was a little light on work because I was out of town for a few days. However, the most of my reading time was devoted to learning about the `NumPy C-API` and how it connected with the `Python C-API`.

I discovered that a NumPy array's primary container object was the [`PyArrayObject`](https://numpy.org/doc/stable/reference/c-api/types-and-structures.html#c.PyArrayObject), and its `PyTypeObject` was the [`PyArray_Type`](https://numpy.org/doc/stable/reference/c-api/types-and-structures.html#c.PyArray_Type). Therefore, in order for any `PyObject` to be regarded as a NumPy Array, it has to fulfil these two requirements.

The API itself offered [functions](https://numpy.org/doc/stable/reference/c-api/array.html#creating-arrays) that allowed any generic array type data container to be converted into `PyArray_Type` or one of its subclasses. For creating the converter, I would always turn to these!