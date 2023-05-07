---
layout: post
title: "My GSoC Journey - Part 2"
subtitle: "Community Bonding"
date: 2022-06-26
background: '/img/posts/community-bonding.jpg'
tags: gsoc
---

## Community Bonding Period
The official [GSoC docs](https://google.github.io/gsocguides/student/how-gsoc-works) describes the Community Bonding Period as *`"The first phase in which you get to know your community and get familiar with their code base and work style."`* So following this definition, my main goal in these few weeks(**20th May - 13th June**) was to get to learn the inner workings of Gnuastro and trying to understand the communication within it, by becoming an active part of the community.

### Week 1
As I had my end semester exams scheduled from 9th May till 27th May, it was tough for me to be as active as I would like, so this week was majorly spent **observing the communication between the different memebers of the community**, mainly through our [Element(Matrix) Channel](https://matrix.to/#/#gnuastro:openastronomy.org). I would also browse through the [Savannah pages](https://savannah.gnu.org/bugs/?group=gnuastro) of Gnuastro to see the current bugs/features in work, while also noting the contributions currenty being made, by reading through the great commit messages.

### Week 2/3 - The First Meet with Mentor !!
I dedicated myself to learning about [Python Extensions](https://docs.python.org/3/extending/extending.html#compilation-and-linkage)(whose docs are really succinct!) and testing them out by creating some sample modules during this period. It was quite inciteful as it gave me a greater perspective into what the final outcome of my project will be and what flow of work I would have to follow to achieve that.
<br><br>
I was also pretty excited about finally getting to meet my mentor, [Mohammad Akhlaghi](https://akhlaghi.org/), who I had only talked to over the mail and IRC, but had been incredibly kind and welcoming. Taking the schedules of all other developers into cosideration as well, we decided to make **`Tuesdays 1:00PM,CEST(4:30PM IST) as our weekly meet times`**. The first meet went great, and was really fun(allbeit overwhelming) to meet all the other developers and get an insight into the awesome work everyone's doing at Gnuastro! I presented a few slides I had prepared to give an overview of my project to the the other developers. Alongwith Mohammad, we were also able to decide a few goals for the next meet, them being:
- Learn more about **`Python Extension Modules`** and try to extend a simple Gnuastro Library module([<u>cosmiccal</u>](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/CosmicCalculator.html) in particular) into Python.
- Reasearch into the [<u>NumPy C-API</u>](https://numpy.org/doc/stable/reference/c-api/index.html) and how we can go about building a converter between [<u>Gnuastro's core data-structure</u>](https://www.gnu.org/savannah-checkouts/gnu/gnuastro/manual/html_node/Generic-data-container.html) and NumPy's [<u>PyArrayObject</u>](https://numpy.org/doc/stable/reference/c-api/types-and-structures.html#c.PyArray_Type).

### Conclusion
As mentioned in the afformentioned definition of *Community Bonding* given by GSoC, I think I managed to get a good insight into the Gnuastro's community by **observing their communications and contributions** while also making myself familiar with their work style!