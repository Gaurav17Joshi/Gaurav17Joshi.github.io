---
layout: page
title: Browsy
description: An autonomous browser agent that drives a real Chrome, with a chat panel living inside the page.
img: assets/img/browsy/browsy_demo_frame.jpg
importance: 1
category: Work
---

Browsy is a browser agent that launches Chrome with its own profile, attaches over the DevTools Protocol, and injects a chat panel and a virtual mouse cursor into every page it visits. You watch it work in a real, visible browser, on real sites, logged into your own accounts if you want it to be — it is not a scraper and not a headless harness.

<div class="text-center" style="margin-top: 2rem; margin-bottom: 2rem;">
    <a href="https://github.com/Gaurav17Joshi/Browsy" class="btn btn-primary btn-lg" role="button" target="_blank" rel="noopener noreferrer">View on GitHub</a>
</div>

---

## How it works

{% include figure.liquid path="assets/img/browsy/browsy_demo_frame.jpg" title="Browsy's chat panel and virtual cursor, injected live into a page" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    A frame from the demo run: researching five open-weight models, reading YouTube comments for sentiment, and building a comparison page, start to finish, in 9 minutes for $3.10.
</div>

The model picks how to perceive a page, per step: **accessibility refs** by default (cheap, precise, survives redesigns), `run_js` when extraction beats clicking, and `screenshot` for canvas apps and other genuinely visual surfaces where there's nothing to read.

Guardrails live in code rather than in the prompt — a domain allowlist, a confirmation gate on irreversible clicks, and a file fence around a single directory — so a model talked round by text on a page still can't get past them.

## Tech

Python 3.12+, the Chrome DevTools Protocol, and the OpenAI API. Tested with 96 panel checks against rendered geometry in real Chrome instances, plus 30 file-access checks for the file fence.

I write more about the design decisions behind it on the blog: [Building Browsy](/blog/2026/building-browsy/).
