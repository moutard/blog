---
layout: post
title:  Scrum was the root of our frustration
description: First I have to acknowledge that I am not a huge fan of scrum. I tried with 5 different managers, 3 different teams, 10 different projects and I have a simple assessment. It never worked for us.
date:   2019-03-15 15:01:35 +0300
author: raphael
image:  'https://miro.medium.com/v2/resize:fit:4800/0*nbK6wfdO8RNTfe-Y'
tags:   [management, agile]
tags_color: '#477690'
---

There are many reasons why it didn’t work and I will detail each of them. But I want to mention the first symptoms of an endemic discomfort: Frustration.

# Three Frustration stages

Before Sprint Frustration: Every single planning we realize that at least 50% of the tasks don’t have a proper description or acceptance criteria. So we argue for hours to define the correct behaviour because the Project Owner is not in the room. Then we have an endless debate during the planning poker to try to get a good estimate that will eventually end-up completely off. We spend hours defining what does “Done” mean for each specific feature. We realized that some tasks will be blocked by another team. We put the task to fit people in the sprint (we have an engineer who only does front-end so we would always put front-end tasks just for him).

During the sprint Frustration: Design would change a requirement during the sprint. “That’s nothing, just the colour of a button”. There is always a production bug that needs to be fixed asap. I previously saw an entire feature dropped in the middle of a sprint. Stand-up is long and not informative.

End of sprint Frustration: 3 days before the end of the sprint, we realize that nothing is “Done” every story is still “In Progress”. So we start getting pressure from Management, and Product Owner. We work long hours, stress increases, we take shortcuts so we can have something ready for the incoming demo… It’s like having a deadline every 2 weeks.
Our burndown chart tells it all, we finish only 60% of our sprint, and we close our tasks the last day of the sprint.

<img src="/images/scrum-root-frustration/burndown-chart.png" loading="lazy">

The only good thing is that we are consistent. Every single sprint is the same, we do the same errors all the time. We are supposed to learn from one sprint to another. I read our last 20 retrospectives and I see the same things mentioned over and over “lack of acceptance criteria”, “disruptive operational work”, “standup is useless”, “too many meetings”, “last minute requirements”.

But we always find a good reason to explain our failure. Insanity is doing the same thing over and over and expecting different results.

# Scrum is the problem

After reading the first part, most of you probably want to tell me “Scrum is not the problem, you are doing it wrong”. And I partially agree with you. If we follow Scrum Guidelines perfectly we wouldn’t have most of those problems.

All stories must have acceptance criteria, all stories must be small, all stories in the sprint are final. Why don’t we just do that? That’s where I think scrum is actually the problem. It requires a list of conditions to be true to actually work. It assumes we live in a perfect and ideal world. Unfortunately, I don’t. My world is messy, it changes often, it has legacy and dependencies that I don’t own.

Scrum doesn’t fit my world. And after talking to dozens of teams inside and outside of my company, I am not the only one with this kind of issues. How can so many clever people fail to implement this methodology? As [Don Norman](https://en.wikipedia.org/wiki/Don_Norman) would say if someone struggles to use a tool, the user is not the issue, the tool is. Scrum is just a tool and if it’s great for some teams, the rigidity of this framework make it counterproductive for us. Now we know the problem. What’s the solution?

# Go back to the Philosophy

I love the root of agile methodology, as it’s described by Dave Thomas, one writer of the manifesto for agile software development. “Take a small step in direction of your goal, evaluate the result, adjust your direction and iterate.” (1)

<img src="/images/scrum-root-frustration/philosophy.png" loading="lazy">

This concept is simple, it doesn’t assume anything. So why scrum became so complex? How did we end up with so many cryptic names and endless meetings? Dave Thomas claims that’s because “Agile” became a business. Making it complex allows companies to sell their books and training. Let’s go back of the philosophy, we want to be flexible to adapt to change. I released a couple of constraints to make it more effective for us.

# Don’t optimize for Time

The notion of fixed length sprint came from an interesting idea. Every small step has the same size. But it has 3 strong negative effects.

First, why a sprint should be 2 weeks long? The adjective agile means “able to move quickly and easily” and yet we lock ourselves in a 2 weeks sprint. Priorities, requirements, nothing can change during a sprint, everything is frozen for a dictated period of time. Change can only happen at the end of a sprint. It’s like saying we are only agile between sprints (i.e. one day every 2 weeks). The philosophy we mentioned previously doesn’t say anything about “fixed size step”.

Second is even worse. My team started to optimize for time. During Poker planning, points inevitably became days. When we had a couple of points left, we would pull another small and unrelated task into the sprint. As those features were small and independent they were usually done by the end of the sprint, but not the main ones who would really bring customer value. Moreover, as our management wanted to produce more, they started to plan 3 or 4 sprints ahead. That is against the concept of a small step.

Third and final point. You can only evaluate when the step is complete. You can not measure the impact of something halfway done. I hate the feeling of closing yet another incomplete sprint because we need 2 more days to deploy in Prod.

Our team introduced a different size for a sprint — a 1 feature long sprint. It’s simple. At the beginning of a sprint, we choose with our PO, one feature that will bring customer value. The dev team breaks the feature in tasks and estimate each of them in points or T-shirt size. Then we start working with laser-focus on this feature only. The sprint finishes whenever the feature is done. It could be 3 days or 2 weeks, it doesn’t really matter. But as we focus on delivery only one feature, I can only close the sprint when it’s done. I still have one problem. What does “Done” mean?

# Optimize for Delivery

I realize how hard it can be to have a clean and non-ambiguous definition of Done. We always had questions like, “Should we include integration testing? or perf testing?” “Should we include accessibility or should it be a different story?” “Should it work on every android device”. You will still to have some of those discussions with your team, I think it’s more a discussion about acceptance criteria. But here is how we fixed most of our problems. We defined the notion of Done to focus on delivery.

A feature is done if it’s used by multiple external customers without any error.

I don’t want to hear, “the code is pushed so it’s done”. If it’s not released it’s not done. If it’s released but no one is using it because it’s behind a feature gate, it still not done. I like this definition because it implies 3 things:

* you deploy often (CI/CD).
* you monitor errors.
* you aggregate usage metrics.

Part of the acceptance criteria should be a dashboard or a SQL query for the number of active customers using this function, number of errors, latency. If you can get from the PO a list of KPI to evaluate success that’s great, and you should include them to your acceptance criteria. For instance — We are changing the contrast of the Buy button, we are expecting sales to increase by at least +3% to consider it a success.

I want to acknowledge that it’s hard to get those KPI. Sometimes a feature doesn’t have such a visible impact on a short time window. Your sales could be impacted by an external factor. But I want to make the point that you need a minimum set of metrics.

The point of my definition is that it’s loose. It respects the Agility cycle. You need to release to take a step, and you need metrics to evaluate.

# Embrace the unexpected

Once we defined what we are trying to achieve with our Product Owner, it’s time to take a small step. Faster you deliver, faster you evaluate, faster you adjust. But I can guarantee, something wrong will happen.

If a requirement changes, incorporate the change. That’s where I don’t understand scrum. You add something in a sprint, and you realize the customer will not validate it. Why would you keep working on it? It’s like telling me, I am making a step in direction of the wall. You’d better change direction now. Just make sure to log those last minute changes and size the impact on the delivery.

If a bug impacts your production, stop your sprint to fix it. Operational issues are more important than new features. If the website is down, the customer impact is so important. You have to fix it quickly. Stopping your sprint allows you to put as much resource as needed to fix it.

Take your time there is no more end of sprint deadline. The focus is on delivering just one feature, focus on quality, long-term, and keep in mind your main goal. The entire team is working on that. It’s obviously only valid for small teams 3–4 dev.

# Conclusion

I have been pretty critic about Scrum. I want to reiterate. It’s not all bad, it just doesn’t suit us. We tried many times, it made us slower, and increased the frustration of our dev team. I found contradictory the name Agile with such a rigid process.

That’s why we fixed our problem by going back to the root of the [manifesto for agile software development](http://agilemanifesto.org/):

“Individuals and interactions over processes and tools”, “Responding to change over following a plan”. We worked to understand the philosophy of agile development. Let’s sum up by:

* Think about the Philosophy — Agility Cycle: Make a step, Evaluate, Adjust.
* Don’t optimize for Time — One feature per sprint.
* Optimize for Delivery — Customer centric definition of Done
* Embrace the unexpected — It’s ok to disrupt a sprint.

We emancipated our team from the process, and focus on the philosophy Maybe you mastered Scrum with your project and your team. In this case, you should obviously keep it. But if you ever relate to the frustration I described, you may want to try to relax some of the constraints.

(1) Agile is dead — Dave Thomas