---
layout: post
title:  5 years on-call, Lessons Learned
description: It’s 2h07 am, I am sleeping when the loud alarm noise of my pager rings in the room. I wake up in seconds, my eyes are dry, and my back is sore. I take a look at my phone. The screen is blinking with an error message, “Database - CPU is reaching 80%, higher than the 70% threshold.” Looks bad. Something is probably hammering our database. What I didn’t know yet, is that it will go worse and my night will be short…
date:   2019-01-28 15:01:35 +0300
author: raphael
image:  '/images/on-call/night-screen.png'
tags:   [technology, management]
tags_color: '#477690'
---

I worked for two types of companies. The first one had dedicated sys-admin in charge of deploying in the Production environment and were on call. Then I joined Amazon where software engineers had to be on-call for the software they produce. I first thought it was strange, but I realise now how critical this can be.

I’ve learned the hard way that on-call is essential. But when I am in front of new hires in Amazon here are the 3 main reasons I use to explain why it’s essential. They all relate to Amazon Leadership principles, but easily applicable to everyone.

# Customer Obsession

First, when something breaks, it means your customers are impacted and are not able to perform a valid action. For us, the database storing the inventory of an entire warehouse was overloaded. It was slow to respond, impacting some pages of the retail website. I woke up and checked our monitoring tool, in the last 4 min, 350 customers got an error when clicking on the checkout button, preventing them from purchasing. If this ever happens to your website, I can guarantee that you will want to fix that specific bug quickly.

This issue has a noticeable impact on your business. So if you are pragmatic, it makes perfect sense to page people who understand more about the code. They know what changes were made recently, they can quickly identify the root cause of the error and deploy a fix. I realised that 87% of traffic was coming from one single client. At 2 am, when the database reaches 95% CPU and is on the verge of collapsing, I didn’t think twice, I changed the security group of our RDS instance to block this IP, hoping this would contain the event.

In this high-pressure situation, you have to stay composed to make the best decision for the customer. You also don’t have time to request access or approval, so to remove friction points, give production access to your developers, that’s why Amazon believes in Dev-Ops.

# Ownership

Second, if you are responsible for the code you ship, then you’ll think twice before pushing any line of code into production.
My first job was in a small company, I remember this Friday night, I’ve been working on a feature for a week, and my manager comes to me and asks me to push in prod so the customer can test during the weekend. If you’ve ever been on-call, you know releasing on a Friday night is a bad idea, because if something breaks nobody will be there to help you. Unfortunately for me, I didn’t know that yet, so I just followed the order, pushed and carelessly left the building… As you can guess something went wrong during the weekend, but I wasn’t on-call, so I didn’t suffer from it.

As I was investigating this spike in traffic in the middle of the night, another alarm went off. There were 5000 errors in of one of our microservice that provides an API for stats on the inventory. Using a tool like Sentry, I was able to parse the error log in real time and read “ERROR timeout: cannot connect database”. I found my guilty client hammering our database when I blocked its IP it started to fail. I looked at the code and saw we made a change on the retry logic of our API that was deployed one day before. Rollback to the previous version was just a click on a button. But why did it start hammering our database at 2 am and not before? I still have no clue…

Ownership means if it breaks it’s on you. To be clear the point is not to blame you but to make you responsible. Don’t put dirty hacks in your code, because it will fire back. Invest in test coverage, automation, CI/CD, monitoring real-time logging and alarming because you want to know precisely when something goes wrong. The idea is to feel the pain of every bug. If you do, you will have clear priorities. On-call gives you the power to fix what hurts.

# Insist on highest standards

Third, if there is a problem (and trust me there will be one), you will fix the error once and for all. You don’t want to be paged twice for the same reason.

So far it’s 3:30 am, I manually changed a security group, made a rollback to an older version of the code, customers can make purchases again, and the stats API is working, but I still have no idea what the root cause is, why did it start failing at 2 am, why did the test didn’t catch this behaviour. Let’s be honest it’s just a quick and dirty fix. But that’s ok because the next day the whole team invested time in a proper solution. It started with identifying the root cause, a problem bug in stats API code, we decreased the timeout of the database client, and put a retry logic. In a nutshell (1), an external client used the stats API to generate large financial reports requiring warehouse stocks. It was running them every week at 2 am. Because the query was CPU consuming it would timeout and retry without killing the previous process.

We wrote a test to reproduce the bug deployed a proper fix and monitored the deployment closely. We wrote a post-mortem, this document describes in many details the event, lists the customer impact, compiles appropriate data. We used the 5 whys method to trace the root cause. Came up with a list of action items to avoid this in the future. It serves as a reference and documentation. Post-mortem documents are reviewed monthly by the teams and can help everyone across the company. It took us 2 weeks to write a good post-mortem document, and implement all the actions items related. Like a throttle for internal and external clients, organising drill, putting in place chaos monkey system etc…

Moreover, you’ll make sure you are prepared before any incident. We have a dedicated onboarding process before an on-call. We have a document called a Runbook that describes routine procedures in case of a problem. Based on the idea of checklist it makes sure we don’t forget anything in case of an emergency. And trust me at 3:37 am, it’s easy to forget something.

# Conclusion

There is no “small bug” or “minor error”. They all hide something more significant, a lack of quality that will one day pile up and backfire. When I joined the team, we used to be paged on average once a day everyone was suffering from that situation, we were spending more time stopping fire than working on new features. But as our manager was on-call too, he was keen on fixing this situation. After one year of aggressive investment on testing and best practices, we reduced the number of alarms to once every two weeks. On-call can be stressful, but it’s the best learning tool for software engineers.

(1) NB: I simplified the story of my on-call, what happened was more complex and harder to detect. I didn’t want to flood the article with too many irrelevant technical details. The main idea is the same, and the conclusion remains identical.

Disclaimer: This is a personal reflection that doesn’t engage any of my or current or previous employers.