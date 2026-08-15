---
layout: post
title: Making Vision Agents
date: 2026-08-15 12:00:00
description: Notes on what it takes to make agents that reason over the images they encounter, instead of reasoning around them.
tags: agents vision ai
categories: sample-posts
---

{% include figure.liquid path="assets/img/making-vision-agents/bat_final_render2.png" title="A fruit bat modelled and textured in Blender by an agent, rendered mid-flight" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    The bat was built in Blender by an agent (GPT 5.6 luna) that could see: it wrote short Python scripts, ran them in a live Blender session, looked at the resulting screenshot, and corrected itself from there. The full flight clip is [here](/assets/video/making-vision-agents/bat_flight.mp4).
</div>

This blog describes the recipes for what makes visual agents work. By visual agents I mean agents that look at images and act on them, i.e. they reason over the images they encounter during the task, instead of reasoning around them.

---

## Why agents are good at text tasks and much worse at visual ones

Visual tasks are harder than text-based tasks, as these often come with simple verification methods. Examples like code/math have simpler ways to check the quality of the answer.

Visual tasks struggle with this, as ex: "Nothing mechanically decides whether something looks like a bat". LLMs currently lack both visual capabilities and visual intelligence (reasoning in vision space) compared to humans, and that makes it harder to make verifiers. This causes 2 things:
1. Harder to make visual verifiers using VLMs
2. Due to 1, visual agentic tasks are not in the RL training runs of LLM companies, hence agents don't learn how to operate and work in such an environment (i.e. they have learned the path of refactoring a codebase but not work through a visual task)

The consequence is visible the moment you watch a capable model attempt visual tasks. For example: give a frontier model MCP for Blender, ask it to create a scene given an image reference, and it will not use vision at any point. It reasons its way to the answer instead: the mug is eight centimetres tall, so it goes at `(0, 0, 0.04)`, and then the next object, and the next, without ever rendering the viewport or moving the camera to see what it has produced. The scene essentially gets built blind and reported as finished. Structurally the result is often defensible, in that nothing intersects and nothing floats, but it is visually dead, because nothing in the loop ever looked at it.

{% include figure.liquid path="assets/img/making-vision-agents/Blendermcp.png" title="Agent makes large chunks at once" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    Agent (Gpt5.6 sol) makes large chunks at once.
</div>

It would be wrong to describe this as an inability to see. The model sees perfectly well when it is made to. The problem is that it does not spontaneously reach for vision as a way of solving the problem, and on the occasions when it does look, it is not able to properly reason with it (often not having an idea of what it is looking for).

## Some example visual agents

Some examples of visual agents would be:

1. **Placing objects in a 3D scene to match a reference image**, which is the routine work of building simulation environments for robotics. You have a photograph of a real room and you need a sim close enough to it.
2. **Producing a slide deck using Google Slides.** Making decks using action rather than scripting with LaTeX.
3. **Driving GUI software that offers no scripting interface**, such as most CAD packages, video editors and DAWs, where the interface is genuinely the only way in.

In this post, I explain things with my example of building a 3D model in Blender (from a reference image).

## Give the agent a plan before it starts

This follows from point 2 above. Frontier models have no trajectories for this environment, so the model does not know what the task looks like from the inside: what order the parts get built in, how to go about each one, where the traps are, what "finished" means for a piece. It has no sense of what trajectory it should be following, and it will not work one out on its own mid-task — it improvises, at length and with confidence, often in the wrong direction. So the trajectory has to come from us, as a plan handed over before execution starts.

So where does the plan come from? A model asked to plan its own work with no grounding will produce something that reads plausibly and is useless, hence we need to give it material to reason from: primarily **a set (10-20) of worked plans** for other tasks in the same environment, along with documentation for the tools it will actually have. The worked plans are what fix the shape and the altitude a plan is supposed to have.

**What does a good plan look like? And more importantly, how detailed should it be?**

- An over-specified plan disables the agent: if every action is scripted in advance, the agent stops reasoning about the task and starts trying to satisfy the specification, and once reality diverges from your instructions even slightly it has nothing to fall back on, because the plan has trained it out of thinking during that step. It follows you into the error.

- An under-specified plan leaves you where you started, since "build a bat" is not a plan in any useful sense.

What works is stating what should be done and why it matters, while leaving how to do it open (some high level help could be provided). Each step properly names its objective, and indicates which tools are appropriate to it (without giving any parameters etc).

For visual tasks the plan can also carry visual context, and this matters more than I expected. For my example, the bat plan shipped with an annotated version (with JSON coordinates of each landmark) of the reference photograph:

{% include figure.liquid path="assets/img/making-vision-agents/bat_plan_landmarks.jpg" title="The reference photo with every landmark drawn and named, colour-coded by group" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    The reference photo with every landmark drawn and named, colour-coded by group.
</div>

The plan I had for this task was 6 steps, each with a paragraph of description plus a review checklist. It is also the right place for facts that follow from the measurements but that the agent should not have to derive. Mine does not stop at listing landmarks, it also states that the torso is 3.77 times longer than it is wide, that the humerus is 14% of the span from arm root to wingtip, etc.

**Be precise with whatever is described in the plan**

Everything in the plan gets read literally and in full, and anything left ambiguous gets resolved by the agent in whichever direction seems reasonable to it at the time. One case from my example: I gave it bounding boxes over the 4 body regions of the bat as loose crop windows around it, but did not mention that they were loose crops, and the agent, while making the head, made it fit the box instead of making it follow the head from the image.

{% include figure.liquid path="assets/img/making-vision-agents/bat_head_overfit_to_box.png" title="A body profile preview showing the head built far too wide, filling its bounding box" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    A body profile preview showing the head built far too wide, filling its bounding box.
</div>

The agent then did not realise its mistake (even though it had been told to follow the bat's image), and continued its run — ambiguities of this kind would fail the run.

*Interesting thing to note: between a visual signal and a textual signal, if there is a conflict the agent would always follow the textual signal (like the bounding box over the head here).*

**So how do we get these plans in the first place?**

The loop that worked for me, and that I think transfers to other domains:

1. An expert writes a **short** plan for the task (the skeleton, not the polished document).
2. The agent runs it and produces a full trajectory.
3. A human corrects the trajectory wherever it went wrong.
4. Repeat 2 and 3 till the trajectory comes out clean.
5. **Compress the corrected trajectory back into the plan.**

**Drawback of planning up top**

All of this is a bet that the shape of the task is knowable before it starts. For the bat it is (body → limbs → membrane → texturing, and no photograph is going to reorder that).

But for tasks with extreme variability, where you cannot tell how things will shape out 5 steps in, a plan up top is worse than none. The agent keeps following it well past the point where it stopped describing the situation, and a confidently executed wrong plan is much harder to recover from than ordinary confusion. Such tasks need something adaptive (replanning between steps, plans that branch), which I do not have a good answer for.

## Reviewers, and what to review against

Another important feature is reviewing, and as VLMs are not as capable as humans, what we can do is: in each step of the plan have a review checklist, and a separate reviewer agent runs against it. The checklist matters as much as the step description itself, and it also fails in one characteristic way: **vagueness in the objectives.**

"Ensure the object placement is correct" is not a review criterion. Correct how? The reviewer will supply its own definition of correct and it will not be yours. "Ensure the cup sits on the table near the edge, without overhanging it" is a criterion, since there is one thing to look at and one answer to give.

This matters because reviews are fickle. Asked whether something looks like a bat, a model will answer, but not stably (the same geometry passes on one run and fails on the next for no discernible reason). Asked something concrete, it becomes repeatable.

{% include figure.liquid path="assets/img/making-vision-agents/bat_body_silhouette_check.png" title="The built body's outline drawn on top of the reference photo, with measured width stations" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    The built body's outline drawn on top of the reference photo, with measured width stations.
</div>

For my example, the checkable form of "does the model follow the bat" was: the model's outline must not cross out into the background sky. An overlay like the one above, and similar tools given to the reviewer, can make its job easier — we need such methods to augment the vision capabilities of the reviewer.

*The general move is to find a concrete proxy for the vague thing you actually care about, and have the reviewer check the proxy. It is a much narrower question than "is this a good bat", and it still catches nearly every real failure, since on this task almost everything that goes wrong shows up as geometry poking into sky or falling short of the animal.*

Getting this wrong fails in 2 directions at once:

- The reviewer **misses errors a human would spot instantly**, since nothing told it to look there.
- The reviewer **fixates on things that do not matter**, spending the whole review on some topology detail nobody cares about, while the head sits two inches away at twice its correct width.

I.e. you do not get a weaker review of the object, you get a review of a different object.

Another important thing: **the reviewer should be an agent, not a single grading call.** Mine gets the plan, the checklist and real tool access, and it writes its own inspection scripts, rotates and zooms the viewport, crops in on parts and takes fresh screenshots before giving a verdict. Grading fixed renders it had no hand in framing is much weaker, since a large part of reviewing is deciding what to look at.

**Where do the reviews go?** After every substantial sequence of steps, as often as you can afford. The cost of a review is one agent's time, the cost of skipping one is every step built on top of the unreviewed result. In my run a seed cylinder 20% too wide in step 1 makes the whole animal 20% too wide, and no amount of later tapering recovers it, so the check belongs right after that step and not at the end of the build.

## 2 important failure cases that occur in visual agents

(These exacerbate if you use a cheaper model.)

**1. The agent does not check its own work.** The most persistent failure I hit was not bad geometry, it was unexamined geometry: the agent runs a preview, gets the screenshot back, glances at it, decides it is fine and moves on. What fixed it was making the second look mandatory rather than encouraged, i.e. doing everything twice:

*Every preview, overlay or silhouette check must be followed by a second pass at the thing it measured. Redo the action under the same filename, with `# REDO LOGIC: "<what was off, and what you changed because of it>"` as the first line. **This is required even when the first attempt looks right**; if nothing needs changing, still write the comment and say what you looked at and what you compared it against. A step whose action files contain no `REDO LOGIC` comments has not been checked.*

This roughly doubles the time a step takes and is easily worth it. Predicting that a preview is fine is a different act from looking at the result with the previous attempt in front of you, and only the second one reliably catches anything. Requiring the comment even when the check passes is what keeps the rule alive (the moment "it looked fine" is an acceptable output, the check quietly stops happening).

**2. The agent does not write scripts to look at things.** (Especially an issue with cheaper models.) It should have a shell and use it for exactly that: cropping a screenshot with PIL, scanning a column of pixels for the object/background edge, drawing a grid over a render. But left alone it will take a screenshot and squint at it, when what is needed is cropping to the 40-pixel region in question and enlarging it. The fix is worked examples of this in the context prompt (actual scripts it can copy and adapt, not a description of the capability), plus making it save each script to a folder instead of piping them in inline, since the same crop gets wanted again after every fix.

*Vision from a model that can also compute is much stronger than vision alone. "Does this edge cross the silhouette" is a hard perceptual question and a trivial pixel scan — there is no reason to make the model answer it the hard way.*

---

## Should we work in the visual domain at all?

There is a genuine opinion that even visual tasks should be done through text, and it is worth taking seriously. LLMs are much stronger at text, and a lot of visual work already has a textual interface available: MCPs like the one for Blender, text-based interfaces like Codex CUA, or straight-up textual design (a website as HTML and CSS). Where those trajectories exist in the training data the model can learn them, and the result will be faster and more reliable than anything driven by looking at screenshots. If we want human-level speed on creative visual tasks, this is probably the route, since the looking is what costs the time.

The catch is that a model working purely through text has no way to find out that what it built looks wrong. It can satisfy every constraint that was written down and still produce something no human would accept (the blind Blender scene from the start of this post is exactly that).
