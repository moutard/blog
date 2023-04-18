---
layout: post
title:  Developers don't aim too big
description: Have you ever been in a meeting to add a feature to the product, and the meeting evolves into an endless debate in front of a 20 feet wide diagram of the architecture with arrows pointing in all directions?
date:   2020-03-31 15:01:35 +0300
author: raphael
image:  '/images/aim-too-big/lego-knight.jpg'
tags:   [management, agile]
tags_color: '#477690'
---

If you’ve never seen that, don’t worry it will come. That’s not even the most frustrating part of the process. It will become infuriating when you start implementing the feature and you realise you made a false assumption.

At Spendesk we explain this behaviour by our operating principle “Don’t compromise over quality”. I like working with engineers who push for the best possible solution, who anticipate edge cases. But we encourage them to also follow another principle “Ship fast, ship small”.

Fast, Good, Cheap
As a manager, “Don’t compromise over quality” and “Ship fast, ship small” principles are challenging to communicate. A team member asked me: “Have you heard of the Fast, Good, Cheap triangle? Basically, you can not have a fast project with good quality, that will also be cheap”. So according to him, our two operating principles contradict each other.


<div class="gallery-line">
  <div class="gallery gallery--post" style="height: 320px;">
    <img src="/images/aim-too-big/fast-cheap-triangle.png" loading="lazy">
  </div>
    <em>fast, good, cheap triangle.</em>
</div>




I agree with the fast, good cheap triangle, however, there is no paradox here. Everything makes sense when we understand the “ship small”. It’s essential for our company to ship fast. The fintech industry is growing rapidly and our clients are waiting for new features to bring them more value. We want to ship quality because we have a long term vision of our product. However, we know it won’t be cheap. That’s why we ship small, we don’t need to release a Big Bang change on the platform every sprint. We iterate on small features.

I recently found 3 examples where we failed to “ship fast” because we forgot to “ship small”.


# Node.js Upgrade
During one of our technical meetings in December 2019, an engineer mentioned a problem; Node.js would deprecate the version 8 in January 2020 and we were currently using this version. Everyone agreed we should upgrade, so we put the task in the following sprint, a month later, I realised that our prod was still running on node 8. So I tried to understand why. When I asked the engineer, he told me the migration was taking longer because some packages were not ready for node 12. So he had to fork and modify some code.

- “Why do you want to jump to 12? Is there any version in between?”, I asked
- “Oh yes, there is version 10. It would be trivial but I don’t want to do the migration twice (8 to 10, and then 10 to 12). It will be faster to make them in one go.”
- “How long would it take to migrate to node 10?”
- “One day.” replied the engineer.
- “It’s been a month working on node 12 I think you are obviously wrong when you say it’s faster.”

If I understand the logic, it’s actually wrong to think it will be faster in one go. Moving to 10 was indeed trivial, because all our dependencies had a corresponding version, even if node 12 was the LTS version, some packages struggle to keep their version up to date.

The most important issue for me is that you are not solving the main problem. You have a hard deadline to upgrade the version of node.js. If you are late and there is a security breach there won’t be any patch for the version 8. You will have no choice than to urgently migrate to 10. As it’s a major version change there is a risk of non-backwards-compatible changes that could break your prod.

## Lesson Learned
As managers, we tend to delay some of the technical tasks, like refactoring or paying technical debt. We don’t give enough time to developers to maintain their code. So when they have an opportunity, they want to do as much as possible.

# Database encryption
Same example, different team. A client made a security audit of our solution, he found one of our databases was not yet encrypted at rest. He asked us to do it before he joined the platform. Our DevOps had to encrypt at rest an RDS instance. This basically requires creating a new encrypted instance with the content of the old one, then having a couple of minutes of downtime to switch the traffic from the non-encrypted to the encrypted DB.

However, our DevOps also wanted to move to Aurora (easier to maintain, better perf and cost-effective). He said we should move from RDS to Aurora and encrypt at the same time. This operation would avoid 2 downtimes. But two months later we were still discussing the risk of migrating to Aurora. The Aurora migration had become a bottleneck to the encryption task, and by extension to our client. Aurora is without a doubt cheaper than RDS, but once again that wasn’t the problem we were trying to solve. As a company we could afford to pay a bit more for 2–3 extra months, we could also afford 2 downtimes in one month but we couldn’t afford to lose a client. We wanted to solve a security risk, so why add complexity and try to solve two problems at once? We could have just unblocked the customer by encrypting the RDS first and then make the migration to Aurora, without any customer pressure or deadlines.

## Lesson Learned
As managers, it’s essential we remind our team what is the main problem we are trying to solve and why. We should keep a focus on one problem at a time.

# Typescript migration
Let’s look at a fun and easy example for the end. Our codebase used to be in javascript and we had a huge project to migrate everything to typescript. During this period, developers also took time to refactor the codebase (including changing the import system and splitting big files).

One engineer did all of them in the same commit. The problem is that git is not clever enough to realise you are just copy-pasting code around. Instead, it will see a file deleted and two new files added. So you don’t see the refactoring easily.


<img src="/images/aim-too-big/github-file-deleted.png" loading="lazy">

<img src="/images/aim-too-big/github-file-fixme.png" loading="lazy">


The developer should have done three different commits. The first one to move to typescript, the second one to split the code into multiple files, and the last one to refactor. If he had done this the Pull-Request would have looked like that:

<img src="/images/aim-too-big/github-file-typo.png" loading="lazy">

It becomes obvious, a typo was made during the refactoring. This bug was released and impacted 34 customers. We spent approximately 1 day for 2 engineers to figure it out. But it would have taken a couple of extra hours for the dev to make three commits instead of one.

# Conclusion
Trying to do everything at once will always add risk and complexity. Inevitably this will slow you down in the long term. Do not try to aim too big, just to save a couple of hours. Ship fast, and ship small.