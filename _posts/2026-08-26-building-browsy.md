---
layout: post
title: Building Browsy
date: 2026-08-26 12:00:00
description: An agent that drives a real Chrome, with a chat panel living inside the page.
tags: agents browser
---

{% include figure.liquid path="assets/img/browsy/browsy_demo_frame.jpg" title="Browsy's chat panel and virtual cursor, injected live into a YouTube search results page" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    A frame from the demo: Browsy mid-task, its chat panel and cursor riding along inside a real Chrome window. The full run — researching five open-weight models, reading YouTube comments for sentiment, and building a comparison page, start to finish — took 9 minutes and cost $3.10. <a href="https://github.com/Gaurav17Joshi/Browsy">Repo here</a>.
</div>

Browsy is a browser agent I've been building: it launches Chrome with its own profile, attaches over the DevTools Protocol, and injects two things into every page it visits — a chat panel and a virtual mouse cursor. You watch it work in a real, visible browser, on real sites, logged into your own accounts if you want it to be.

It isn't a scraper and isn't a headless harness. The panel rides along through navigations, the cursor moves the way a hand does, and the whole thing stays visible while it happens.

---

## How it looks at a page

The model picks how to perceive the page, per step, rather than being locked into one mode:

- **accessibility refs** — the default. Cheap, precise, and survives redesigns since it isn't reading pixels.
- **`run_js`** — for when extracting or comparing something is easier than clicking through it.
- **`screenshot`** — for canvas apps, dense grids, anything genuinely visual where there's nothing to read, only something to see.

That last mode is the interesting one, and the one I expect to matter most going forward — accessibility trees run out exactly where the hard tasks begin. Slides, spreadsheets, diagram editors, drag-and-drop games are all canvas or near-canvas. Getting that mode to a point where the agent trusts a click based on what it sees, every turn, and cheaply enough to use every turn, is most of what's left.

## Guardrails live in code, not the prompt

A model that spends its day reading untrusted page text is a model that will, eventually, get talked into something by that text. So the constraints aren't instructions Browsy is asked to follow, they're things the code enforces regardless of what the model decides: a domain allowlist, a confirmation gate on irreversible clicks, and a file fence around a single directory. Being talked round by the page doesn't get you past any of it.

## Where it's going

The demo run cost $3.10 and took nine minutes. Both numbers should drop by an order of magnitude:

- **Faster** — most of that time was waiting on page loads and re-reading snapshots the agent had already seen. Caching known-good trajectories, so a repeated task replays instead of getting re-derived from scratch, is the biggest single win sitting there.
- **Cheaper** — three quarters of the tokens were already served from cache. The next step is not sending the whole page at all: trimming snapshots against the current subgoal before they reach the model, and pushing the grounding step onto something small and local.
- **Better at the genuinely visual tasks** — the `locate(description) → (x, y)` step needs to get good enough to trust with a click, on the first try, every time.

Nearer term: file uploads, a date-picker helper (the single biggest failure mode in the benchmark so far), and — held back deliberately, since Browsy reads untrusted page text for a living — a shell tool, once there's a real answer for keeping command execution out of reach of whatever the page says.

If you want to see it run, the [repo](https://github.com/Gaurav17Joshi/Browsy) has the full setup and the [run report](https://github.com/Gaurav17Joshi/Browsy/blob/main/Use_Cases/RUN-REPORT.md) for the demo above.
