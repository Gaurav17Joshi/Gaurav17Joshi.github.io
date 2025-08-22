<!-- ---
layout: page
title: project 1
description: with background image
img: assets/img/12.jpg
importance: 1
category: Work
related_publications: true
---

Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %} -->


---
layout: page
title: Computational Statistics EBook
description: An online Jupyter Book on Bayesian Data Analysis and Parameter Estimation Methods.
img: assets/img/Cstats_project1.jpg
importance: 1
category: Work
---

I created an online book on **Computational Statistics** using Jupyter Book. It's designed to explain the mathematical principles behind key Bayesian Data Analysis and Parameter Estimation methods, complete with code implementations to illustrate the concepts in action. The EBook was used to explain topics and concepts to other students in Probability/Stastics/ Probabilistic Machine Learning Classes.


The primary goal was to create a practical, hands-on resource for anyone interested in the computational aspects of modern statistics.

<div class="text-center" style="margin-top: 2rem; margin-bottom: 2rem;">
    <a href="https://gaurav17joshi.github.io/CompStats/intro.html" class="btn btn-primary btn-lg" role="button" target="_blank" rel="noopener noreferrer">View the Live EBook</a>
</div>

---

## Key Topics Covered

The book delves into a variety of essential topics, providing both theoretical background and functional code. Key areas include:

-   **Random Variable Generation**: Techniques for sampling from various distributions.
-   **Monte Carlo Methods**: Including Monte Carlo Integration for numerical approximation.
-   **Markov Chain Monte Carlo (MCMC)**: Detailed explanations of Metropolis-Hastings, Hamiltonian Monte Carlo (HMC), and Gibbs Sampling.
-   **Gaussian Processes**: A non-parametric approach to regression and classification.
-   **Information Theory**: Core concepts like entropy and Kullback-Leibler divergence.
-   **Sequential Monte Carlo (SMC)**: Also known as particle filters.
-   **Graphical Models**: Representing probabilistic relationships between random variables.

---

## Project Showcase

Here is a glimpse of some of the visualizations and code snippets included in the book:

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/Cstats_project2.jpg" title="Acceptance Rejection Sampling Method" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/Cstats_project3.jpg" title="Hamiltonian Monte Carlo Sampling" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    On the left, a visualization of the Acceptance-Rejection Sampling Method. On the right, Hamiltonian Monte Carlo Sampling in action.
</div>