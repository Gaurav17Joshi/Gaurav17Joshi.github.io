---
layout: post
title: Building FluenTalk
date: 2025-06-15 12:00:00
description: Notes on building and shelving a voice-based AI startup for practicing spoken English.
tags: startups voice-ai product
---

{% include figure.liquid path="assets/img/Fluentalk_project2.png" title="FluenTalk's practice interface, offering debates, passages, and feedback on grammar and speech" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    FluenTalk helps you speak English fluently with AI, offering practice across debates, passages, and more, with feedback on grammar and speech.
</div>

One of the most meaningful things I've built is [FluenTalk](https://fluentalk-frontend.web.app/), a voice-based AI platform I co-founded after my undergrad to help people improve their spoken English.

---

## The idea

Speech is a different form of communication than reading or writing, which is what most language teaching courses actually train. You don't translate word by word when speaking, you think in the language and respond in real time. Traditional methods like memorization or repeating pronunciation drills don't train this skill effectively. The only real way to improve speech is by having meaningful conversations, starting from broken sentences and gradually moving to more complex dialogue.

FluenTalk was built around this insight. By early 2025, voice LLMs had gotten good and fast enough to simulate real, meaningful dialogue. So instead of static lessons, we built a system users could actually talk to: a voice-based AI assistant that gave feedback on grammar, fluency, and clarity as they spoke, aiming to feel like a real conversation while still providing structured guidance.

## Building it

I worked on the full system end to end: a Django backend calling the OpenAI APIs for the conversational AI, a React frontend, CockroachDB for data, and Firebase for hosting.

{% include figure.liquid path="assets/img/Fluentalk_project3.png" title="Live conversation interface where users talk to the AI assistant in real time" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    The real-time conversation interface.
</div>

More than the stack, though, a lot of the work was about how the experience felt. Speaking a new language is uncomfortable, so we wanted FluenTalk to feel low-pressure and encouraging rather than evaluative.

## What we learned

We tested FluenTalk with over 200 students in a school. One of the most meaningful parts for me was watching students look up the score the AI gave them on their conversations and try to one-up each other. It was the first time I'd built something people actually used and got something out of, not just a system, but something designed around how people actually learn.

We ended up discontinuing FluenTalk once I started applying to grad school and other commitments took over. It didn't lead to immediate success, but it left me with a lot of clarity on what I want to do going forward: build products people actually want to use, and pay for, not just publish papers about.
