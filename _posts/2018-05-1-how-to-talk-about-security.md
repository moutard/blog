---
layout: post
title:  How to talk about Security
description: I realize that the word “Security” is not really glitzy. It’s a topic difficult to approach, as many people think it’s dull or too complex, they don’t feel concerned. That’s a mistake.
date:   2018-05-01 15:01:35 +0300
author: raphael
image:  '/images/how-to-talk-about-security/safe.png'
tags:   [security, devops]
tags_color: '#477690'
---

Security concerns everyone, not just Software engineers. I tried many times to bring awareness about information security to a wider audience, and I realized how challenging it can be. After many years I started to notice that a couple of things make it easier. Here are the top 5 tips to help you make Security more engaging to everyone. Because an attacker always target the weak link. And I have bad news, the weak link, it’s probably you.

# 1. Start with Social Engineering
Most of the security talks I attended usually start with an horror story. Usually, a security breach that went terribly wrong like [Heartbleed (2014)](https://heartbleed.com/) or The [Dyn Attack (2016)](https://en.wikipedia.org/wiki/DDoS_attacks_on_Dyn). But those examples are hard to grasp, especially for non-technical people. The audience members think they can’t do anything about that, so they shouldn’t worry.

That’s why I always start with a Social Engineering example. They are so much easier to get. For instance, here is a funny “Elevation of privileges”

<img src="/images/how-to-talk-about-security/fake-oscar.png" loading="lazy">
<em>Mark David Christenson with his fake Oscar</em>

Mark David Christenson, managed to sneak into the Academy Awards ceremony in Hollywood, using just a tuxedo and a fake Oscar Statuette. He fooled the security guards, they let him in without asking any question. He also managed to get a free meal because the owner thought he was serving a VIP. This is what we call social engineering attacks.

In this scenario, Mark is the Attacker, the ceremony is the Asset and the Security guard is a Vulnerability. Now you can transpose this story to computers. Let’s say the Attacker (Mark) wants to access someone else bank account (The Asset) using the Bank Website (The Vulnerability). If the attacker to look enough like someone else he can impersonate his victim and access his account, that is called Spoofing. You could even do simpler to get a free meal.

<div class="gallery-line">
  <div class="gallery gallery--post" style="height: 320px;">
    <img style="height:320px" src="/images/how-to-talk-about-security/twitter-diner.jpeg" loading="lazy">
  </div>
    <em>(Credits: Kyle Baldinger)</em>
</div>



Social engineering attacks are extremely frequent and powerful. Like hackers exploit a bug, attackers exploit human cognitive biases. Think about bait or more elaborate phone call to gain access to personal information. If you phone enough random people in a company claiming to be the technical support, you will eventually reach a genuine employee with an issue. Those examples help the audience to relate as we all received a phishing email at least once.

# 2. Offer Free Candy
Would you use a free USB memory stick, offered during a conference as a goodie? Would you give your email to access a free service? Would you use the free WiFi in a Café?

Nothing is free! All those actions are potential threats. We all know now that many free online services sell personal data under the hood. Same for a free flash drive. Research found at least 45% of users will plug an unknown flash-drive into their computer (1). Problem is, flash-drives can contain malware, or worse could destroy your computer. A device called usb-killer, looks just like any flash-drive, but it inverts high voltage into the until it burns your motherboard. You can build your own or just buy one online for $15.

<img src="/images/how-to-talk-about-security/usb-killer.jpg" loading="lazy">
<em>USB killer (Credits: dark purple)</em>

Free Candy is a technique used by attackers to attract potential victim into a trap. When you give your email address it can be used to send you phishing emails. When you connect to an insecure WiFi, attackers can intercept all the data you send, it includes authentication token, that can be then used to impersonate you and access your account.

Free Candy is a successful technique used by hackers. So, why not use it to attract innocent co-worker into a security conference.

<img src="/images/how-to-talk-about-security/phishing-license.png" loading="lazy">
<em>(Credits: https://xkcd.com/1694/)</em>

# 3. Be Creative
Security breaches are so frequent it’s easy to find examples. Just look at the [Checkpoint Map](https://threatmap.checkpoint.com/) to see all the attacks happening in real time. Finding an interesting or fun example is harder. Here are a couple of examples I like to use, as they show how creative hackers can be.

In January 2018, a map was released showing whereabouts of people using fitness devices such as Fitbit. Some U.S. military were wearing those devices during their jogging, revealing the location of a sensitive U.S. army facility. [(2)](https://www.washingtonpost.com/world/a-map-showing-the-users-of-fitness-devices-lets-the-world-see-where-us-soldiers-are-and-what-they-are-doing/2018/01/28/86915662-0441-11e8-aa61-f3391373867e_story.html)

In 2013, a [FOIL](https://www.nysed.gov/new-york-state-education-department-foil-requests) request (Freedom of Information Law) was made to release Yellow cabs data (3). The dataset contained details about every taxi ride in New York for the year 2013, including the pickup and drop off times, locations, fare, tip amounts, as well as taxi’s license and medallion numbers.

<div class="gallery-line">
  <div class="gallery gallery--post" style="height: 320px;">
    <img style="height:320px" src="/images/how-to-talk-about-security/taxi.png" loading="lazy">
  </div>
    <em>Bradley Copper taxi trip in 2013 (Credit: neustar)</em>
</div>

From that data, a researcher was able to get valuable information. For instance tracking celebrities. On this picture posted on Twitter by a tourist, he could read the cab plate number, combined with the picture timestamp, he queried the dataset to find the corresponding trip to get the destination.

Just by looking at the drop off address, he was also able to identify men who spent a night at a Gentlemen’s club in a fairly isolated location in Hell’s Kitchen.

# 4. Make it a Game

<div class="gallery-line">
  <div class="gallery gallery--post" style="height: 320px;">
    <img src="/images/how-to-talk-about-security/crossword.png" loading="lazy">
  </div>
    <em><a href="https://zed0.co.uk/crossword/">zed0 crossword</a></em>
</div>

In 2013, a lot of Adobe password’s hash leaked with all their hints. If I give you list of hints can you find the corresponding password? Inspired by an [xkcd](https://xkcd.com/1286/) comics, someone turned that into a crossword game, to show how easy it could be to crack it.
hints for number 8 are: *daughter - dog - grandson - 23 - basketball - michael - boy - bball; mj - basket - granddaughter - bulls - last name - country- sun - shoes - mike.*
I already filled the response for you.

Can you find the number 4 given the following hints?

* **4**: *food - candy - yummy - sweet - dulce - favorite food - brown - favorite candy - cocoa - dark - tasty - coco - dessert - favorite flavor*

If you liked this game, you can find more crosswords grids here [(4)](https://zed0.co.uk/crossword/):

Gaming can be really addictive, the interaction of the game helps to remember the content. Once I wanted to demonstrate how easy it was to access useful information. So I organized a small game, I called that the “Speed Dating Game”.

I would divide the audience into 2 equal groups. To group — called group A — I just say they will participate a speed dating event. They will meet different people for 5 minutes and they just have to introduce and have a small talk.

But for the group B, the briefing is different — I assigned them a secret goal: to get one of this information: The address of the person in front of them, the name of their pet, the name of their parents or the high school they went to. This information can be so valuable. A pet name is a common password, the name of the high-school is often question to recover a lost password.

![Play your opponents](https://source.unsplash.com/Iq9SaJezkOE/h320)

If you just ask “what’s your address?” You will never get an answer, worse the person in front of you will probably be upset or suspicious and could leave. It’s the same with servers, a stupid brute force approach could raise warnings and alarms you could be blacklisted, and will not get another chance.

You need to be subtle, learn your target and use what they say against them. Security is really easier than you think, you could even try to be more hands-on, play with a key-logger or contest to a Capture the Flag competition for beginners like the InfoSecInstitute one, Hack this Site or WarGame. Just make sure you are doing it on a safe and legal space.

# 5. Think like a Hacker
I know it’s a lot to digest. Security concepts are convoluted and sophisticated. So here is my last piece of advice for you.

Think like a Hacker!

Be smart, think out of the box, find the loophole in everyday life situation. [Michael Larson](https://en.wikipedia.org/wiki/Michael_Larson) is notable for winning $110,237 by memorizing the patterns used on the Press Your Luck TV show. He also found a bank giving out $500 for every new checking fund. He used fake names to open dozens of accounts, waited the minimum necessary duration, then withdrew the money. On another occasion, he registered a business under a family member’s name, hired himself as an employee, then fired himself to collect unemployment benefits. (As a reminder both are fraud, so don’t do it).

<div class="gallery-line">
  <div class="gallery gallery--post" style="height: 320px;">
    <img src="/images/how-to-talk-about-security/mr-doodle.png" loading="lazy">
  </div>
    <em>Mr Doodle in the streets of London</em>
</div>

I worked with many security experts but I learned more from meeting a couple of hackers. They don’t obey by the same rules. For the same reasons I started an immersive exploration of the Street Art Scene in London. Street Artists are hackers in some way. Where you see a wood fence, Mr Doodle, a street artiste, sees a white page. I took this picture really early in the morning, where London streets are empty. Obviously never do something illegal, I am just saying you should try to perceive different opportunities and keep an open mind.

Security threats change fast, hackers are usually ahead of you because they think out of the box. Hackers spend time trying to attack, as engineers try to defend. But you have to understand how the attack works to defend it properly. Like chess you want to anticipate your opponents move.

It’s easy to understand that stealing credit card numbers can be used to get money. But that’s not the only way. Have you thought about billions of pounds in loyalty points embezzled (5), airlines company miles stolen [(6)](https://media.ccc.de/v/33c3-7964-where_in_the_world_is_carmen_sandiego), or private information about a company’s IPO.

Finally it’s not all about money. It’s also about Power, think about fake news, information about future voters to influence elections etc…

# Final Words
Once again security is everyone’s responsibility. Engineers can build the most secure website, it doesn’t matter if your users write their passwords on a sticky note visible from their desk. Rarely are fraudsters actually hacking into people’s accounts or stealing money from their cards, instead they’re simply tricking us into giving our money away. They’re relying on the weakest link in bank’s security which, sadly, is often customers themselves.

*Disclaimer: This is a personal reflection that doesn’t engage any of my or current or previous employers.*
* (1) IEEE Symposium on Security and Privacy — Matthew Tischer
* (2) U.S. soldiers are revealing sensitive information — Washington Post
* (3) Riding with the Stars: Passenger Privacy in the NYC Taxicab Dataset — atockar
* (4) Adobe crossword
* (5) Fraudsters are exploiting Britain’s billion-pound loyalty point market
* (6) Where in the World Is Carmen Sandiego? — Karsten Nohl