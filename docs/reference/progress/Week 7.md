---
layout: page
title: Week 7
permalink: /progress/week7
parent: Progress
grand_parent: Reference
nav_order: 7
---
# Progress Report - Week 7

### General info
**Week**: 7
**Phase**: 2 - Research and Experiment + 3 - Component Development
**Contacted**: Bart, Simona, Bernd-Jan, Frank
**Document(s) handed in**: -

##### Biggest accomplishment
I managed to get help on multiple fronts, AR and validating my designs so I could go forward and also managed to seperate my git repository into seperate ones.

##### Biggest setback
I am still quite stuck in some aspects, for example my EDA I am having trouble getting trends from my data, and the AR for now is still 'location based' so based on a GPS location instead of the phone location.

### Daily progress
![Test Image](basic-weekly-template.png)

##### Monday
On Monday I started by prioritising my tasks and figuring out what I would need for the poster market so I would be able to validate my designs and such during the expo. I also asked Frank to open a Feedpulse as I noticed that I hadn't documented my feedback there in a while. Then I started working on the basics for [[Personas]] and User stories so I would be able to set up my questions for a survey for during the expo more targeted.

In the afternoon was the poster market and I talked to Bart, Simona and Bernd-Jan on my current progress and struggles with the project.

##### Tuesday
On tuesday I worked a little bit more on my EDA, but got stuck on trying to decompose the time series. I wanted to be able to observe the trends and patterns of a single location, but it is not recognizing the 10 minute frequency (as in it doesn't recognize T which is the naming convention for minutes) even if I make it so there is no 'missing' data by forcing an entry every 10 minutes and imputing the data just to get some initial insights.

I also worked on AR as I had a meeting with [[28-3-23 Bernd Jan]] later in the day. Originally Simona was also going to be joining us, but due to scheduling issues this didn't work out. We decided that seeing as AR is very device specific in whether it works properly or at all, I was going to borrow an AR ready device at the ISSD. We also have an AR kit there, but the phone in this was borrowed by someone else so to start we were going to use the newest available phone. Bernd-Jan set up the wifi for me, and informed me that there are phones available with data in case I need it later on. We checked whether his working prototype also worked on this phone, which it did, and decided that the phone was stable enough for AR

When I got back to my workplace, I checked again, and despite being closer to the location, I wasn't able to see the red cube anymore, so I decided to get back to it the next day. In the evening Frank opened the feedpulse I requested and told me that he would swing by tomorrow to chat over a drink.

##### Wednesday
On Wednesday Simona checked in on me (online) to see whether the meeting with Bernd-Jan was enough or I needed extra help to continue. I told her that I was going to try and get something working on my own using the new phone, but that yesterday the cube wasn't working when I got back but this morning it was. So I was going to experiment a bit to see whether I could make a rough prototype or that we needed to continue looking. She also mentioned a Fontys XR event on the 12th to me, which I could present at or get inspiration from or just use for validating a prototype and reiterated the fontys IT event on the 23rd of May which I would get an invitation for soon and the tiny digital twins presentation on June 1st.

I also filled in the feedpulse, as I hadn't done that in a while. I also updated my general overview of which aspects I worked on which week and how much.

After the knowledges sharing session Bart had some Media S6 students come by to give some feedback on my project. The rest of the day was spent acquiring, noting down and processing this feedback.

##### Thursday
On Thursday I started by documenting the feedback from the media students more properly in the [[Feedback report]]. I also documented my survey for the poster market as I did do the prep work, but not enough people showed up. I also had a meeting with Teague and Guido about the portfolio blog project. After that I reconvened with Tessa as she couldn't make it to the meeting to discuss what was said and our opinions on this. After that I started working on my EDA again. 

##### Friday
On Friday I started by testing whether my way of deploying (python http.server and linking it using ngrok) worked, by using the code from Bernd-Jan which was working.

After that, I started working on seperating the git repository. I wanted to create a group for it, but right now that wasn't possible with my git.fhict.nl permissions. So I made 3 repositories, Front-end, Back-end and documentation. In front-end, the focus will be on the AR, so the development of the application. The back-end is focused on the data, obtaining, analysing and predicting with it. Documentation is just the general documentation for the internship. I will ask Simona whether she can create a group for me at our next meeting. I also created a workspace in Gitkraken with these repositories (as well as the old one) for easier monitoring.

I also uploaded the previously not in the repository testing files for HTML AR and the Unity React combo. I have not yet found a way to share the unity project as the gitignore currently just removes all the directories in the project.

After that I started going through all the documents that are currently relevant (project plan, research document, feedback report) and fixing their formatting. Then I went through all of my project reports and referenced the documents/meetings etc wherever I could so I could see more easily when I talked about what.