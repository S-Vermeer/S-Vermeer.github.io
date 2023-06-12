---
layout: page
title: Week 11
permalink: /progress/week11
parent: Progress
grand_parent: Reference
nav_order: 11
---
# Progress Report - Week 11

### General info
**Week**: 11
**Phase**: 3 - Component Development  
**Contacted**: Bart, Simona, Frank
**Document(s) handed in**: {XXX}  

##### Biggest accomplishment
I managed to get my pipeline deployed for the front-end using docker, so now if I want I can automatically update the front-end, create a new image and push it. It is also now on a public domain and CORS is fixed, so it is accessible to everyone that can display AR on their phone.

##### Biggest setback
Due to a national holiday and a holiday coming up I wasn't as productive as I would have liked. I was also struggling quite a bit with questions that I couldn't get answers to until I spoke with Simona on Wednesday.

### Daily progress
![ProgressWeek11 Image](/assets/Week11-progress.png)

##### Monday
On monday I started the day by talking to Bart. He was asking how it was going and I mentioned that I was trying to figure out how much a fine dust particle weighed. After talking about this, he mentioned that it might help to prepare things for the knowledge sharing sessions, as it might help me think differently. If I prepare it so it is explainable to my colleagues, it might help make things clear as well as make it easier to ask for help. He also said that I needed to watch the balance of doing proper research and actually doing what I needed to do, as acknowledging your limitations is also part of the learning process. I also mentioned that I believed figuring out what particles weighed was a proper internship research in itself, so figuring out how much effort to put into it was quite difficult. 

After that I continued working on creating plots for our conversation with TNO. Somewhere during that, my brother opened up the subdomains for the city digital twin projects on my domain, so I started trying to get those to run. However, there were some issues with the firewall and SSL certificates so I had to pause that until I could get the domain for the firewall from him, as I could no longer find it. Later I got the domain but I still couldn't get it to work so it needed to be postponed.

I continued working on TNO, and also wrote an email to Simona informing them of my progress and things I thought we should send to TNO, and about my progress on the weight of particulate matter.

In the evening, my brother was able to help me further so we could set up the cdt.skylervermeer.nl and cdt-api.skylervermeer.nl. There are still some CORS headers, but different ones and at least now we could start testing using a https connection.

##### Tuesday
On Tuesday I experimented with CORS, and after a lot of troubleshooting (hours and hours), it worked. We now have connection with the API, currently still a static call but eventually we will have it linked to the location of the user. Based on that the PM1 concentration amount is spawned in models, however this will be updated later.

After that I tried to get the CI/CD pipeline working again, and after a while and a lot of troubleshooting I got it to work. With a whole lot of commits in a seperate branch, I got it to work so it builds and pushes the image, and pulls the image, removes the previous container and deploys the new one.

I still need to add code reviews.

##### Wednesday
On Wednesday I worked on trying out different models and had a meeting with [[26-4-2023 Simona]]. I also checked in with Frank.

##### Thursday
Thursday was a nationwide day off

##### Friday
Friday was a day with limited productivity, as the holidays were about to begin. I did work a little bit on the weight of fine dust, but essentially, not much happened.
