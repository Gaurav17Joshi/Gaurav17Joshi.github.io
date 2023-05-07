---
layout: post
title: "Finguitar"
subtitle: "Our First tunes at a Hackathon"
date: 2022-12-31
background: '/img/posts/finguitar/first.png'
tags: hackathon
---

*My experience in participating in my very first dev-orieted hackathon and winning amongst 150+ teams with a poor implementation of a novel idea and all the learnings that came along with it.*

## What are Hackathons
As suggested by the portmanteau, Hackathons involve hackers doing a marathon (never a real one of course :wink:), basically boiling down to clever programmers engaging in rapid engineering and creativity over a short period (usually 24 hours). Hackathons are the perfect battlegrounds to test out your skills or develop new ones on the go. The schedule of a hackathon usually goes something like this:

![Register(usually in teams of 2-4) -> Idea Formation based on certain broad domains -> Discuss the viability and intricacies of the idea with a mentor(optional) -> Application (usually in the form of a web or mobile app) -> Demo the idea and project in front of a jury]({{site.baseurl}}/img/posts/finguitar/flow.png)


Our first hackathon was [Code Odyssey](https://codeodyssey.tech/) organized by KJSIEIT, Sion. It involved over 150 teams (of 2-4 members) and took place on Feb 12-13, 2021. This blog is about our experiences(and mistakes) in going from hackathon noobs, to somehow winning our very first hackathon(and still being noobs after that).

## Brief Intro about us
Before participating in our very first Hackathon during the tail end of our Third Semester of Computer Engineering, we were just a bunch of Second Year students, with little to no experience in competing on such a wide stage.

We wish we had some elaborate planning involved in how we formed our team, but it was as simple as being four friends with varying interests and experiences who wanted to work together. How did we know about each other's varying interests and skills? Well, all four of us were a part of the [Inheritance Mentorship program](https://www.communityofcoders.in/events/631f4cddf8688e4aaec19d39) organized by the [Community of Coders, VJTI](https://www.communityofcoders.in/). [Jash](https://github.com/Jash-Shah) and [Pratiksha](https://github.com/psankhe28) from team Neo and [Sarvagnya](https://github.com/saRvaGnyA) and [Alisha](https://github.com/alisha-kamat) from team Star. It was through observing each other's team throughout this program that the prospect of combining our talents interested us and thus, team NeoStar Aaghadi was formed! (don't ask where the 'Aaghadi' came from)

Having participated in the Inheritance 2021 program by CoC, all four of us had dabbled in web dev, but we would be lying if we said we were ready to take on a 150+ team hackathon.

In hindsight, we probably should've prepared a bit before the hack, like seeking out generic templates based on potential ideas, having a tech stack decided and a default backend app ready to be deployed, etc. but it being our first hackathon and all, we went in blind and naive. So kids, **do as we say NOT as we do!**

## Idea Formation
After having convinced ourselves that somehow it is a good idea for four complete beginners to participate in a 150+ team hackathon with students from the Third and Final year(some of them being our Inheritance mentors :eyes:) we had reached the night before the hackathon starts.

This seemingly awkward and insecure "council" session to try to come up with an idea that is *creative, original, has business value, positive impact, and has long-term use* turned out to be one of the most fun and creative parts of the hackathon. I mean, it's you and your friends brainstorming about esoteric and niche ideas, acting like you have **any of the capabilities** required to create them, but **believing fully** that you have the next Facebook on your hands :upside_down_face:.

The name of the game here is being *innovative, creative, and open-minded*. This is also the best opportunity to form healthy communication between your team, which will reap long-term benefits down the line:chart_with_upwards_trend:. While just regurgitating previously successful ideas as long as they meet the theme requirement is a viable strategy in a Hackathon, **there is little learning to gain from it**.

The idea for Finguitar came by combining two whacky ideas, one from Sarvagnya which involved a sort of a keyboard piano app where you can play different musical instruments using your keyboard, and another project which was trending at the time of a hand gesture detection web app for blind people. We combined the two to come up with - ***Finguitar - A web app that uses Computer Vision to build an online instrument that you can play using just your fingers :musical_note:!***

## Work Division, Communication, and Time Management
The time crunch of a hack not only tests your individual coding skills but also *how well you can work **in and as a team***. It is not always necessary that the athletes best at a 100m sprint also excel in a 1000m relay. We quickly learned to ***divide and conquer!***

Following our hackathon timeline, we spent the first hour exploring the Discord server and presenting our idea to our assigned mentor. *Validation is the fuel for productivity*, and that is exactly what our mentor gave us along with a few tips on how to mold our very rough idea into a viable product :memo:.

Our Tech Stack involved:
- **React-JS** for the frontend,
- **Flask** for backend,
- **OpenCV+TensorFlow** to handle gesture detection.

We segmented our efforts based on domains. Working with web development was comfortable for Sarvagnya and Pratiksha, so they handled the ReactJS part. Jash felt "confident" with the backend involving Flask and the ML model. Alisha worked on the acoustics as we were "supposed to" construct an entire instrument.

After dividing the work, we all went into our separate work hibernation pods, while still being online on discord to be able to hop into a call whenever required.


## The Thrill of Learning on the Fly
*“I am a person who works well under pressure. In fact, I work so well under pressure that at times, I will procrastinate in order to create this pressure.”*

All professional procrastinators(engineers) are more than privy to those last-day study sessions before exams, where we make up for all the bunked lectures and ignored chapters of the portion. How are we able to do it? How are we able to learn a topic that has been escaping us for an entire semester in just a few hours? **"The pressure of the deadline"** is the answer.

It's this exact pressure cooker environment that is created and motivated during a hackathon. It enabled us to not only learn things on the fly but apply them to the project at hand as well!

While there were a lot of "fun and interesting:sweat_smile:" obstacles we faced, integrating the gesture recognition model with the web interface was the sore thumb. We had to spend around two to three hours just to learn an entirely new protocol called [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) and then apply it in both ReactJS and Flask to be able to - Take a frame in the browser, emit it on a websocket, detect a gesture, and then relay that gesture back to the websocket.

Despite having taken out a big chunk of our time because of some over-complications made in dealing with the [Base64](https://en.wikipedia.org/wiki/Base64) encoding of the frames on the backend by Jash, it taught Jash and Sarvagnya some valuable debugging skills while also bettering our communication along the way.


## Final hours and Project Demo

The frontend from Pratiksha, the gesture detection API from Jash, and the acoustics from Alisha all needed to be integrated by Sarvagnya the night before our first round of evaluations with the mentor in the morning:alarm_clock:.

*Only when all the ingredients are correctly incorporated into a pizza is it delicious.* A browser had to be used to reproduce the challenge of creating an appropriate instrument experience. Beyond connecting the parts and tuning the user experience for three hours after midnight, we produced a cute little prototype. At about three in the morning, Sarvagnya excitedly submitted a demo video of this to the team members. Jash (supposedly awake of the mess he feared Sarvagnya would make) saw the video and approved it.

The mentor evaluations the next morning went smoothly with us nailing the demo. We got qualified for the top 10 :tada:.

## Final Pitch
This is the **make-it-or-break-it moment**. No matter how much of your original vision you were able to achieve (or not achieve in our case), you have to believe in your idea 100% and sell it as so. At the end of the day, most dev-oriented hackathons are incubators of new ideas than a test for your technical skills. As long as the latest version of your project is *not broken enough* to be used as a proof of concept, you need to pitch every feature you've added and can potentially scale as part of your demo.

We had never previously presented our concept before. We were unsure of what to do. Our prepared slides and demo were both overly technical.

We got the opportunity to witness the other teams' demos as ours was in the 7th slot. The judges, who were big shots from successful tech startups, made their expectations abundantly clear based on their ***lack of interest in the technical details of the idea but more so in its business acumen***. Even our Inheritance mentor Ravi's team wasn't spared when their entire presentation spiraled into a gabby discussion regarding the use of blockchain and web3 in their proposal(they did eventually manage to win a sponsor track prize in the hackathon). We also realized that our project was way less technically impressive than others who not only had a fully featured web-app but a mobile-app to boot.

It was finally our turn, we readied our slides, and Alisha started with the pitch. The judges seemed intrigued by the idea, but the interest died down as Jash started explaining the backend. Suddenly, *one of the judges turns on their mic and asks us to skip all the technical slides*!? That meant **skipping 10 of the 16 slides** we had made:grimacing:.

So with only 2 slides left, and a yawning judge panel, we basically started improvising a pitch. Talking about how we had built a new & first of its kind online instrument, its viablity in malls as a demo for real instruments, its impact on education when used as a learning aid in schools, etc. Instantly, the panel became active and interested. They even started brainstorming ideas with us!! Primarily stressing the commercial viability and gamification of our idea managed to impress the judges.

<iframe width="560" height="315" src="https://www.youtube.com/embed/rfgN-wgJye8?start=8059" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Results

We'll be truthful. We did not anticipate qualifying as one of the Top 10 teams. Hell, we did not even anticipate qualifying the initial idea demo to our mentor. However, after giving our pitch and evaluating the judge's enthusiasm against the other teams, we felt certain that we could secure at least one of the top three positions.

Finally, with biting nails and bated breath we heard the results announcement, and seeing "Team NeoStar Aaghadi" written alongside 1st Place Winner sealed the deal :sparkles:

![Winner Slide]({{site.baseurl}}/img/posts/finguitar/result.png)

## Conclusion

In conclusion, if a bunch of newbie Second Year hacksters can somehow manage to fool a distinguished judge panel into selecting their under-developed novel idea above 150+ other teams, then there is no reason for anyone to not try their hand at a hackathon!


## References
- Team Members:
    - <u><a href="https://github.com/Jash-Shah">Jash Shah</a></u>
    - <u><a href="https://github.com/saRvaGnyA">Sarvagnya Purohit</a></u>
    - <u><a href="https://github.com/psankhe28">Pratiksha Sankhe</a></u>
    - <u><a href="https://github.com/alisha-kamat">Alisha Kamat</a></u>
- Our Inheritance Projects:
    - Team Neo: Jash and Pratiksha: <u><a href="https://github.com/Jash-Shah/Data-Map">Data-Map</a></u>
    - Team Star: Sarvagnya and Alisha: <u><a href="https://github.com/toshan-luktuke/stock-market-analyser">Stock-market Analyzer</a></u>
- <u><a href="https://codeodyssey.tech/">Code Odyssey Hackathon</a></u>
- <u><a href="https://github.com/Jash-Shah/Finguitar">Finguitar</a></u>