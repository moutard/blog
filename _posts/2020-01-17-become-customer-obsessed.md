---
layout: post
title:  How I became customer-obsessed
description: For my first week at Amazon, my manager sent me to pick boxes in a warehouse. I travelled to the countryside early in the morning and started my shift like every other warehouse worker, but I was a software engineer.
date:   2020-01-17 15:01:35 +0300
author: raphael
image:  '/images/become-customer-obsessed/warehouse.jpg'
tags:   [management, customer obsession]
tags_color: '#477690'
---

My team was developing the software running the warehouse and optimising the storage space.
At the end of the week, my manager asked me:
- “Who are our customers?”
- “The client who gets their delivery” I replied without any hesitation.
- “Unfortunately you are confusing our end-users and our customers. Our end-users actually don’t care how we store stuff but warehouse workers do. We work for them.”

I was not always customer-obsessed, I learned it from Amazon, and it took me years to feel confident talking about it. Some developers are so passionate about the technical challenges that they forget about users. I think that’s a mistake. Everyone should care about customers.

# Why does it matter?
Every year, Axios publishes the Harris poll, a report that ranks the reputations of the most visible companies in the United States. Amazon was ranked 9th in 2008, and quickly moved up the ladder to the top 5 and stayed there for 12 years.


<img src="/images/become-customer-obsessed/fang.png" loading="lazy">
<em>Source: Axios Harris Poll 100</em>

According to this report, Facebook dropped 43 spots last year, falling from #51 in 2018 to #94 in 2019. The main reason is the Cambridge Analytica scandal. People realised that Facebook sold 87 million users’ personal information without obtaining proper consent. This private information was then used to target potential voters and influence elections. Google also took a hit and lost 13 spots from #28 to #41 due to the Maven project. An initiative financed by the US Department of Defense to apply AI technology on military drones images. There is one crucial lesson from this data.

> “It takes years to build trust and only minutes to lose it.”

Overall it was a lousy year for GAFA, but Amazon is still 2nd of the ranking. So they must do something right, I believe the difference is due to one of their Amazon leadership principles “Customer Obsession”.

# The tech for the tech

Before joining Amazon, I co-founded a startup in 2011. Our mission was to push relevant web content recommendations based on anonymous data. (i.e. you get recommended content without the ads targeting you). That was great. We worked on a machine learning algorithm that would respect privacy by design using encrypted data. We wanted to build a recommendations system based on a peer to peer protocol so we wouldn’t centralise personal data on our privately own data-centers. As an engineer, it was one of the most challenging problems of my career. I was coding and learning all the time. The company had some success at the beginning, but the project never took off.

The truth is that our product was not solving any customer problem. We though privacy was an issue, but in 2011, it was not. I saw so many dies because of the lack of customer value.

# Revenue vs Customers

A startup needs fund to survive. We ran out of it after 2 years. Don’t get me wrong: rentability is not a good indicator of customer obsession. We need to invest money on research, even if not financially viable. Apple did that well in 1980, investing in expensive R&D resources for their new MacIntosh. Despite conflicts with the board, Steve Jobs kept his customer-centric vision.

On the opposite, making money doesn’t mean you are customer-obsessed. Some companies are happy to extort their client for financial gain. (You can learn more by listening to this great episode of “Dark pattern” by Gimlet).

[Gimlet: Episode 144 Dark Pattern](https://gimletmedia.com/shows/reply-all/6nhgol)

We need to make sure we don’t measure success only with money. NPS, CSAT scores are also important. But the most important metric, you get it by talking to your clients.

# Customer Obsession (the easy part)

After learning the basics of customer obsession within the warehouse team, I joined a team working on Alexa. The voice assistant was first considered a gadget, Amazon doesn’t think that way.

## 1/ Work backwards

Always start by the customer. Before writing any line of code, Amazon writes a Press Release, a one-pager describing the value for the customer. It can be long term value, but we want to understand what problem we are solving.
When Amazon introduced the Echo, it had great reviews, but customers found it slightly expensive. Amazon leveraged this input to release a cheaper version. The press release insists on the price because it’s the main benefit based on feedback.

Introducing the All-New Echo Dot-Add Alexa to Any Room for Less than $50 | Amazon.com, Inc. — Press…
The new Echo Dot is hands-free, voice-controlled, and powered by Alexa-play music, turn on the lights, get information…
press.aboutamazon.com

## 2/ Measure everything

Don’t make assumptions for your customers, don’t trust your gut feelings, measure their behaviour. Your decisions should be data-oriented.
Implement your vision, but measure like you are trying to prove you wrong. For instance, in our scrum process, the definition of ‘Done’ includes KPI we want to measure to check our assumptions. When we released a new feature on Alexa, we knew it would be advertised on TV. So the metrics “reach 1 Million users in the first 2 weeks” was not relevant. To prove the success of this feature we wanted to measure “Number of customers reusing the feature more than 3 times a month for a 6-month window”. Don’t measure the success of your marketing campaign but measure real adoption.

## 3/ Be close to your customers

Amazon requires you to be on-call. The first time you get a page at 2:00 am, you start to understand what customer obsession really means. It helps you realise that you work for clients. I describe more my experience as on-call in another article.

[5 years on call: Lessons learned](/blog/5-years-oncall)

It forces me to aim for quality. We had a bug, a client called the support because he said “Alexa buy me 2 bottles of wine” but received 3 bottles (1). The team investigated the problem and worked on a fix, then told me the bug was solved. I realised they just pushed the commit to the test environment. I challenged them saying: “If it’s not released in Prod it’s not done.”

I encouraged them to do the extra mile for our customers. Once it’s in production we should the customer it’s solved. I also asked them to refund the extra bottle, some people argued that was the job of the customer support but I told them CS didn’t introduce the bug, we did so that’s the least we can do.
Finally, I asked them to list all the clients who had the same issue but didn’t realise yet because their order hasn’t arrived. Proactively contact them and refund them. You don’t want to be customer-obsessed only for people who complain.

## 4/ Use persona

Jeff Besos is known to bring an empty chair to every meeting. It represents the customer. “What would our customer say if he were in the room?” Many startups now use persona to help them identify groups of customers, and focus on solving their specific problems.

# Customer Obsession (The hard truth)
Press Releases, Metrics, On-call, Persona, are just tools. Yes, they will help you at the beginning, unfortunately becoming customer-centric is much harder. A persona is merely a caricature of a typology of clients. A customer is someone I can talk to, someone I am trying to help, and I can relate to. So here is my secret:

> “Treat everyone as a customer.”

During a presentation, I said that to my colleagues, “We are not friends, we are not colleagues, you are my customers.” That was a controversial statement. I stand by it. As a software engineer, I am paid to deliver a service. I am a supplier. If they can find a better alternative, they will replace me. Many companies do that by hiring freelancers to release faster or outsourcing development to be cheaper.

Every time someone comes to me with a technical question, I answer like he is a customer and I internally think “this discussion can be recorded for training and quality purposes”. That’s how customer-obsessed I am.

Don’t take me wrong, it’s a good thing because I care about my customers, and want to help them. Some people were not convinced. So I took an extract from the engagement survey the company runs every year.

<img src="/images/become-customer-obsessed/engagement-survey.png" loading="lazy">
<em>Extract of the engagement survey.</em>

Whether you like it or not, my friends do not rate me on a scale from 1 to 4, only my customers do. If you consider everyone as a customer, your mindset will change, and you’ll make better decisions.

You will quickly learn that it doesn’t mean the customer is always right. You don’t have to say yes all the time. That’s not a healthy relationship with a client.

We had a customer pushing for a feature. He was the only one. We tried to understand which problem he wanted to solve and then realised that the functionality doesn’t align with our product. We couldn’t help him with our solution, so we helped him migrate to a competitor.

It’s better to say no to a client that doesn’t fit your value proposition. You can help more your customers by saying no. Priority will become more evident.

# It's still Day 1

In his first Shareholders letter, Jeff Bezos said Amazon’s mission was to become the “Earth most customer-centric company”. His goal was not to build a retail website. His goal was to focus on the customers and provide them with a service.

And every year, he repeats “That’s still Day 1”. In other words, we are not there yet, and there is still a lot of work to do. It became such a sharp moto that one of the first Amazon building in South Lake Union campus in Seattle (HQ of Amazon) is named ‘Day 1’.


<div class="gallery-line">
  <div class="gallery gallery--post" style="height: 320px;">
    <img src="/images/become-customer-obsessed/old-amazon-logo.png" loading="lazy">
  </div>
  <em>This is the first Amazon logo. The letter A carved by the shape of the Amazon river.</em>
</div>

*Disclaimer: This is a personal reflection that doesn’t engage any of my or current or previous employers.*

(1) That’s not exactly what happened, I just want to avoid complex details to explain the problem.