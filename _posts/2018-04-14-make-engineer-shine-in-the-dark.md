---
layout: post
title:  Make your engineer shine in the dark.
description: Have you been to a play recently? A concert? You probably only remember the main actor or the lead singer. But actors are not enough to make a play. You need costume designers, make-up artists, sound engineers. Think about everyone involved and working backstage.
date:   2018-04-14 15:01:35 +0300
author: raphael
image:  '/images/shine-in-dark/light-room.jpg'
tags:   [management, devops]
tags_color: '#477690'
---

I have been working in the Software industry for 9 years, and I noticed something. Software engineers like to work on shiny projects. They want to work on the new React Native mobile application, the Deep Neural Network recommendations engine. But they stay away from No-SQL migrations or refactoring a legacy stack. They want to work on projects they can show and brag about. In other words, they like to be on stage.

A software is like a play, successful projects require people working in the dark. It may feel unrewarding, but I learned to embrace those opportunities.

## Backstage Projects are customer impacting
I have been in a drama company for more than 6 years. I had the chance to be both on stage and backstage. I especially remember my experience as a lighting technician. You spend the entire show in a small room with no light. It’s behind the audience, with a console that controls the spotlights. See the irony, you control the lights but you work in the dark. Your only contact with the stage is a small tinted window. Literally, no one can see you. At the end of the show, the audience applause the actors bowing, not you.

<img src="/images/shine-in-dark/dom-juan.png" loading="lazy" alt="types to schema">

But light is an essential design element of a play. It can change the scenery. Look how the spotlights focus on the main characters and hide the rest of the scenery. White cold lights outline the warm yellow lantern. Without the technician, the stage would stay in the dark. All the work done backstage is important for the customer. It’s the same in the software industry.

In my career, I worked on many of those “Backstage projects”. I transferred an entire infrastructure to the cloud. I migrated MySQL database to No-SQL. I split a monolithic app into micro-services. To value this work you have to understand why I did it, and why they were customer impacting.

Yes, the customer doesn’t care if you use cloud solutions or physical hardware. However, migrating to the cloud, allowed us to scale down at night when traffic was low. In 6 months we saved 25% of cost on hardware. It also made our deployment process easier, saving developers time. Instead of releasing once a week we moved to continuous deployment. That’s valuable for our customers, they get features quickly. Once we were in the cloud our old MySQL database was growing way to fast. We were using the biggest cloud instance available. Replication was a nightmare, engineers were paged 10 times a week because of issues related to the database. Migrating to No-SQL was a challenge. But we were on the verge of collapsing. So it became a priority. The successful migration allowed the service to scale by 10x.

By now you should be convinced that backstage projects are important. They make your platform more stable. They allow faster iterations. You pay legacy debt and avoid future costs. For each “Backstage “ it’s usually easy to identify how they bring value to your customer. Use as much data as you can to your claim.

## Embrace the challenge
Those projects are ambitious. As your service is used by customers you cannot afford downtime. Try to change the engine of your car as it’s running on the highway. Every heavy migration requires proper planning. An engineer operates dry-run. The lightning technician rehearses with the actors. An engineer builds script to migrate automatically. A lightning technician leverage console automation. One on a button and it runs a pre-recorded sequence of light events. This avoids manual errors.

<img src="/images/shine-in-dark/maintenance.png" loading="lazy" alt="types to schema">


It may not feel as fancy as working on a new framework or a cool feature. But they represent exceptional challenges. They move you out of your comfort zone and make you better a better engineer.

You have to think about what can go wrong. Anticipate and mitigate known unknowns. In my team, we call those projects “Operational work”. They include support, on-call, refactoring, paying technical debt. We take ownership really seriously. If our team pushes a feature live, it’s our responsibility to maintain it. That includes hardware provisioning, metrics alarming. Checking spotlights before the show, and replacing faulty ones is crucial. And for some reasons, software engineers tend to despite this maintenance work.

## Enjoy the shadow
As I was trapped in this dark lighting room during a show, I learned a last important lesson. There is only one reason that will make you visible. If something goes wrong.

Imagine Benedict Cumberbatch in Hamlet at the Barbican Theatre. The audience is captivated by the famous monologue. When all the spotlights turn off. You can be sure, everyone will look around for a guilty technician.

It’s a team effort. The main actor trusts people working backstage to play their part. Same for a heavy migration. Nobody cares until you break the production server. You have to do it without impacting the delivery of new features. If you do your job correctly nobody will ever know. There is no individual reward for doing it right. Your job is literally to stay invisible. As an engineer, we should enjoy being in the dark. As a team, we should recognize lightning technicians, as anonymous heroes. They are a crucial part of our workforce. As a manager, we have to help them achieve goals and grow as engineers. We want to make their life easier, and praise even dull operational work, that contribute to a healthy product and happy team. If you do that you may start to see your engineers glow in the dark.

* Photo Credits: Dom Juan returns from War https://www.flickr.com/photos/sam_js/2052778912/ (Creative Commons)
* Photo Credits: Light replacement https://www.flickr.com/photos/manuelfreiria/14734674072/ (Creative Commons)
* Disclaimer: Disclaimer: This is a personal reflection that doesn’t engage any of my or current or previous employers.
