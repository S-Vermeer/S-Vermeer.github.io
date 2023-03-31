# Progress Report - Week 2

### General info
**Week**: 2  
**Phase**: 1 - Problem Definition  
**Contacted**: Simona, Frank  
**Document(s) handed in**: Stakeholder - Project Plan and Research  

##### Biggest accomplishment
I practically finished my project plan. There might be some minor points of feedback, but I now have clarity and structure, also partly due to my scoping that I feel confident about is not too limiting but also allows me to have some breathing room in case of significant setbacks.

##### Biggest setback
The past data for fine dust is not easily accessible. You can download it, but if you want to download all of them it really messes with the columns. However, you can seperate which ones you download, but you can only download in batches of 70 days. There is an API but I have not found a way yet where you can get past data so  I also can't automate the process (yet). So I either need to manually select and wait for 58 locations to download and do that in sets of max 70 days, or I need to figure out a way for the API to do it automatically or figure out a way to fix the columns.

### Daily progress
![Daily Progress week 2](Week2-progress.png)

##### Monday
On Monday I started by setting up a FHICT gitlab repository and transferring all my current research on there so it was easily accessible and I could more easily share the markdown files I was creating in the note taking program I am using, Obsidian. This because I noticed that once I converted them to PDF it didn't look as clean as I wanted, and some of the links weren't working. So I decided to invest some time to set up both a gitlab repository that I would be using later, as well as converting some of my previous markdown as some references only worked in obsidian but there were also multifunctional references that would work in both Obsidian and the Gitlab markdown view. I didn't update all links, just the documents I was working on at that moment.

Next I updated some parts of my last progress report that I hadn't filled in yet, as I didn't summarize some of the feedback I got. After I did that, I started transferring the information I gained from my city problems research to answer the second research question and adding things such as APA referencing.

Then I started working on setting up a basic app for my proof of concepts by copying a previous React App. I removed the unnecessary parts and added screens for all three predicted proof of concepts that I could navigate between.

I also filled in my feedpulse for the previous week.


##### Tuesday
On Tuesday I was mainly working on getting in order the things I would need to make a decision about a city problem on Thursday while waiting for feedback on my project plan that I knew was coming in the afternoon. That is why I started working on my knowledge sharing session presentation as a way of both getting a better overview of my assignment and what I was missing and would like to know more about as well as finding ways to summarize my research to key points.

In the afternoon my project plan feedback came, there were a few pointers but overall it was estimated to be 80% complete. The pointers were some clarifications on the research questions, based on which I changed a research question that I already needed to change based on feedback from my company coach. The other two I don't know how to change yet, as I believe I am already answering them in my current research questions. Another point of feedback was that he wanted to sit with me for the breakdown and time planning.

After that I started experimenting a little bit with accessing the camera and doing any kind of AR in my basic app just to see how difficult it was going to be. 

##### Wednesday
Wednesday morning I had my second knowledge sharing sesssion. During this I discussed my project plan and my current research. Overall they were impressed with what I had done so far, the only note they had was that the scope of 'the general person in Eindhoven' might be too big. From another intern collegue I got the recommendation to narrow it down using age and living style. For example since babies will not be using the application, you could say for example adults who are working a full time job. Aside from the knowledge sharing session I also looked into Augmented Reality techniques but didn't find a source that fit my goal of tracking techniques yet.

In the afternoon I talked to my Frank about my time planning and breakdown. We agreed that I would make a start that we would discuss on Friday. During that meeting we will check whether the scope is still okay and whether the approach is the best approach. Frank advised me to use a waterfall approach for the research with iteration based for the development stages. He also mentioned that Aytaç Sarkmaz did an entire research on Augmented Reality a few semesters ago and that it was worth approaching him to see whether I could use it so I would spend less time on researching AR and more time on creation. He also mentioned that most Augmented Reality projects use paid frameworks, so that I needed to discuss with Simona about my budget for that so I knew what my options are.

Based on this I started working on mapping out my approach and planning so I could discuss this later with both my stakeholder and coach. I also sent a message to Aytaç asking him whether he still had his research on Augmented Reality and whether he could send it to me.

##### Thursday
I started Thursday by adding things like introductions to documents and chapters where applicable. Then I mapped out which fields of the Dot Framework and which techniques I was using, and based on this I chose to use SWOT analysis instead of Multi-Criteria Decision Making  as aside from Literature Study I wasn't using any other Library Technique. Then I started with a basic SWOT analysis of what I found in my research on City Problems so I could discuss this with my stakeholder later. 

In the afternoon I had a meeting with Simona, where we decided that we were going to choose fine dust as our domain. I also validated my waterfall/agile planning approach and my updated vis 2 research question. The budget Frank mentioned on Wednesday she didn't think was necessary as she believed Frank might have been talking about models which we don't need to buy as fine dust isn't very specific, however she was going to look into it. She also said that the target audience should stay wide, so maybe you could say 10+ with a phone but it should be widely accessible. We are going to be using location based AR, so there is no need for markers.

Based on this I started incorportating this choice and the decision making process into my research document.

##### Friday
Friday was mainly spent trying to figure out ways to scrape the data as efficiently as possible as just downloading it wasn't working. I eventually figured out a python loop that went through all the locations and used @odata.nextlink to get the next piece of data. However this took longer than expected and would have to run during the holiday as well.
