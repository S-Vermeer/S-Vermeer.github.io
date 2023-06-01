---
layout: page
title: Process and Result
permalink: /process-and-results/
nav_order: 3
---
## Process and Results
### Structure
To start this project, I set up a structure so I had a clear overview of what needed to be done, a git repository for version control and easy sharing and could also identify areas that I was not sure yet what was expected of me.

For this project, I decided to use [Obsidian](https://obsidian.md/), which is a markdown editor with a variety of features. The initial features that stood out to me were easy linking between documents, graph view allowing me to see how documents (and research) were connected, the ability to install plugins, the ability to create templates for documents easily and by using markdown the files would be easily viewable in git. 

Based on my documentation from previous semesters I created a template {LINK} for my documentation, used them for each of the planned documents, and adjusted them to match the relevant document better. 

I also created a to-do list {LINK} so I had a better overview of important tasks that needed to be done. Later, I moved this over to [Trello](https://trello.com), so I could also filter my tasks and make them more visible to myself. I created 3 boards for this: Documentation, Development and Milestones.  Documentation was to break down the documentation into bite-sized tasks and make sure I process the information such as expert interviews and feedback. It had the sections 'To Do', 'Doing' and 'Waiting on Verification', and 'Done'. I also added tags such as 'waiting on other task', 'High Priority', and 'Need Help' to better identify why I hadn't done a task or which task had priority. Development had a similar setup but was meant more for concrete tasks such as scraping data, testing, developing a feature, designing etc. Milestones were meant to keep me on track, make sure I have all the approvals I need, and all the documents I need, and had tasks such as 'Prediction Chosen'. These tasks are paired with the phases they are associated with, as described in the approach section.

For the Git Repository, I used the FHICT Gitlab, allowing my company coach and assessors easy access, easy display of markdown and easier transferring if the project continued. Initially, I had a single repository, however, I noticed that if I was experimenting and wanted to work in branches, this meant I could not for example do my documentation because the experimentation branch was multiple commits behind the experimentation branch, and if I had something that was running for a while, I couldn't just switch branches as that would remove the progress. To fix this issue, once the progress allowed for it, I set up 3 repositories: cdt-a-i-r-zoom-lens-documentation {LINK}, cdt-a-i-r-zoom-lens-front-end {LINK} and cdt-a-i-r-zoom-lens-back-end {LINK}. This way I would always have access to my documentation, and my front end, consisting of Augmented Reality and such was separated from my back end, consisting of my data analysis and modelling. These repositories were also shared with my company coach, Simona, so she could see my progress and do her own experimenting when needed.

### Problem definition
The next aspect was to define the problem. To start this, I transferred my project proposal {LINK} into my Project Plan {LINK}. Based on this I knew I needed to scope down my project to a single city problem. That is why I did initial research to answer the questions:  "Which city problems are being monitored in Eindhoven?" and "What impact do these city problems make?". For this, I did literary research which was documented in the Research Document {LINK}, specifically 1.1 and 1.2. I started the research off by looking at which problems were noted in the [Eindhoven Open Data](https://data.eindhoven.nl/pages/home/) portal, and connecting these to their origins: Population Growth and Urban Growth. Next, I started looking more into the impact of these problems and documenting this. Based on this research, I made a SWOT analysis of the different problems to see which would be the best to scope our project on.

![Fine Dust SWOT](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/swot/fine-dust-swot.png) 
![Biodiversity SWOT](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/swot/biodiversity-swot.png)  
![Air Temperature / Water SWOT](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/swot/Airtemp-water-swot.png)  
![Air Pollutant Emissions SWOT](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/swot/air-pollutant-emissions-swot.png)

Based on this, together with Simona, I decided that Fine Dust was the best choice to move forward, as this was the one with the most potential for success and would incorporate AR and AI the best. The complete breakdown of this decision-making process can be found in the conclusion of chapter 1 of the Research Document {LINK}.

{I was planning to make a requirement prioritization as indicated in the project plan, but during the research the priorities kept shifting as we discovered more and more things that we needed clarification on etc so I resorted to just discussing the current priorities with my company coach during our weekly meetings.}

### Theoretical / Domain Research
Now I knew what problem I was going to be tackling, I was going to need more knowledge on a variety of subjects. Particularly, Augmented Reality and Fine Dust. Though there was enough information to choose Fine Dust as the scope, to be able to make it more tangible more knowledge on the subject was needed. As for Augmented Reality, more knowledge was needed on what it was and what choices needed to be made. That is why Simona invited Jean-Paul Close to join our project as an expert. After talking to him, I documented my findings in the Research Document, under 1.2 What impact do these city problems make? {LINK}, and noted him as the source. 

Based on the recommendation of my first assessor, I contacted Aytaç Sarkmaz for his research on Augmented Reality so I could scope the different types of augmented reality and the different frameworks and platforms. I combined this with more literature research on Augmented Reality in general and on the types where applicable and documented this in the Research Document, under 2.2 What is the best Augmented Reality technique for this application? {LINK}.

Later on in the project, after the key development decisions were made {Describe how literature research or smth was used to answer 4.1 What devices will the AR application be available on?  }

### Practical Research
As for the more practical research, I used several methods. I supported the choice of the Augmented Reality technique by using several Proof of Concepts to see which aspects worked and didn't work. These can be found here {LINK}.

I also answered research question 2.1 How can this problem be visualized? in the Research Document {LINK}. For this, I created sketches and discussed them with my stakeholder. After that, I created a clickable wireframe and tested this with Semester 6 Media students. Initially, the goal was to use a survey and test it with the visitors at the poster market, however, the attendance at this event was not as high as expected and thus I had to resort to Media students. Based on their feedback, I updated my designs and then verified them with my company coach and the expert.

Based on these verified designs, I started developing the application using HTML, CSS and Javascript in combination with AR.js and Aframe.
{Creating 3D models}
{Usability testing}

Another important aspect of this project was the data that needed to be visualized.
{describe the process of scraping and preparing initially}
{Comparing to TNO report}
{Other research of that data}

{How can it be verified whether users understand the severity of the city problem? - observation and user testing}

### Results
Presentation, feedback, website etc
cdt.skylervermeer.nl