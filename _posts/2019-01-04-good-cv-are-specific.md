---
layout: post
title:  Good CVs are specific
description: Your CV is the first interaction you have with recruiters. GAFA receive hundreds per week, to stand out, yours should be distinctive and informative.
date:   2019-01-04 15:01:35 +0300
author: raphael
image:  'https://source.unsplash.com/s9CC2SKySJM/h320'
tags:   [management, hiring]
tags_color: '#477690'
---

# What’s the problem?
As part of my day to day job, I run technical interviews for software engineers. I’ve read on average 12 CV per months for the past 4 years. I filter out 15% of them, when the candidate doesn’t match the experience, or profile I am looking for.

After that, I run a 1h technical phone screen for the remaining 85%. I use the content of the CV as a good ice-breaker, it helps the candidate feel comfortable. Once in a CV, I read the following line:

> “I made the interaction faster by re-writing a significant part of the code, leading to better sales conversion”.

At first glance, I thought it was a clear example and I was in front of a strong candidate. But something was bothering me. I only realized what was wrong during the interview when I started to ask:

“You said you made the interaction faster. How much faster?” the candidate told me “10 times faster”. I replied “Do you mean it was 10 seconds before and 1s after or 100ms became 10ms. Because both are 10 times faster”.

Then I asked, “When you say significant part of code, how many lines of code? what percentage of the code base? Were you working alone on this project?” The candidate didn’t really know that, he just told me he was working alone.

Finally I added, “What does better sales conversion mean? How did you measure it?” The candidate said “I’ve been told me we had growing sales after this change”.

That’s when it stroke me, on an 1h interview, I spent 15min probing for details on this single sentence. That’s a waste of time for the candidate. This info should already be in the CV so I can focus on gathering important data, like how they react to pressure, how they deal with unexpected events, how they work in a team.

# Weasel words
After this interview, I wanted to understand if it was just a one time thing or it was something I’ve been missing for a while. So I started reading the latest candidates’ CV, looking for this kind of blurry statements. I spotted things like “I learned various libraries”, “I deployed some services”, “making calculation faster”, “used by major providers”. In the first 5 CVs (3 pages each) I found 32 vague descriptions.

Wikipedia guidelines define those statements as Weasel words. They are used to create the illusion of a specific and meaningful statement, when they only carry ambiguous content. They fall into 3 types:

* Numerically vague expressions (“many clients”, “a lot of code”, “faster queries”)
* Passive voice to avoid specifying an authority (“it is said”, “it’s well known”)
* Adverbs that weaken (“often”, “probably”)

I had saved all the CVs in a folder, so I built a small script to parse for those terms, highlighting weasel words and computing statistics. Those 510 CVs covers experience from 0 to 19 years, front-end, backend, full stack. However it may be biased because I work in London and 78% of them are from Europe with non-native English speakers and 89% are men. I anonymized all the data, exclusively for Software Development Engineers and here are the results.

<img src="/images/cv-are-specific/data-weasel-word.png" loading="lazy">

It concerns everyone, from junior to senior, 95% of the CVs have at least one of those weasel words, 81% have more than 5. I checked my own CV and realized I had 2. But this is not the only problem.

# Peacock words

<img src="/images/cv-are-specific/peacock.jpg" loading="lazy">


Peacock words are used to promote the subject while not imparting verifiable information. (“outstanding technical skills”, “most innovative algorithm”). They can make you look good but presumptuous, so use them carefully. I also gathered some data about them.

<img src="/images/cv-are-specific/most-common-peacock-word.png" loading="lazy">

Peacock words are a double edge swords. You need a couple ones to stand out and add a quality sensation to your experience. Words like “core”, “cutting-edge” “advanced” or “innovative” can be used. But less than 3 and avoid preposterous one like “flawless” or “vibrant”.

<img src="/images/cv-are-specific/vibrant.png" loading="lazy">

If only I knew how to create a flawless architecture and vibrant UI.

# How to fix it?
Easy, data is the solution. As you can see most of the weasel words are part of numerical category. Every time you see a weasel word, try to replace it by real numbers. Let’s take a look at 2 examples:

> “All the customers were impacted by a main issue, during this event we lost a lot of money. Hopefully, I fixed the problem really quickly.”

Reading that, I want to ask: How many customers? For how long? What was the impact? What was the root cause? Instead I would write:

> “10 400 customers couldn’t complete a purchase for 2h13, due to the server failure introduced by a code change, resulting in an estimate loss of $50 000 (+/- 4%).”

The second phrase is only 9% longer (14 characters) than the first one. But bring so much information. It’s also based on facts and I can make my own opinion, is it really a “quick resolution” or a “a lot of money”. Let’s take another one:

> “I drastically improved performance of the most popular page, by using cutting-edge technologies and innovative solutions.”

Once again “cutting-edge”, “innovative”, it sounds good but there is no way to verify this information.

> “I reduced the homepage p90 latency by 25% from 800ms to 600ms, by compressing html through babel and caching recommendations in memcached, increasing views on mobile by +10% from 5000 to 5500 per day.”

Numbers can be vague too, and data can be presented in the wrong way. You want to stay precise, use boundaries and percentage — 10% faster from 10s to 9s. Use exact number, do not round up — 10 400 not 11 000, unless you use shorter form like 10K.

# Storytelling — Be specific

A CV is just the storytelling of your career, a document “in which YOU are the hero”.

> “Once upon a time in a distant location, someone was doing something pretty intriguing.”

> “May 1789, a royal soldier is running through Paris with a sealed message for the king.”

Which story do you want to read?

Both sentences are 85 characters long. With the same amount of characters, the second manages to transfer 4 times more information as it answers the following questions: when? where? who? what?

Candidate with less than 3 weasel words in their CV perform 62% better during the first interview, when their CV is specific. It’s only a correlation, and I can’t prove anything with a sample of 510 CVs. However I understand that in 3 ways

* Specific CVs give me more information so I can focus my energy on more valuable questions.
* Specific CVs are easy to remember for me, they stand out.
* Candidates are used to communicate more precisely.

Good stories are specific, your CV should be too. Software Development Engineers are using the same vocabulary over and over. Out of 510 CVs some words are used a lot.

<img src="/images/cv-are-specific/most-common-word.png" loading="lazy">

This is true for every section of your CV. For instance, 42% of them do have a section Hobbies or Interests. But avoid vague activities like “Travel”, “Food” and “Music”, it doesn’t really help me to know who you are.

<img src="/images/cv-are-specific/hobbies.png" loading="lazy">


# Take away
* Every document you write should be specific from CV to articles.
Use facts and data to backup your message.
* If the data contradicts your message, change your message not the data.
* Avoid presumptuous peacock words.
* Be specific in your hobbies too.

*Disclaimer: This is a personal reflection that doesn’t engage any of my or current or previous employers.*