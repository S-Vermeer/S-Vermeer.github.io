---
layout: page
title: Portfolio
permalink: /documents/portfolio
parent: Documents
grand_parent: Reference
---

# Portfolio - A(I)R Zoom Lens
{ TITLE PAGE }


## Document History
### Version

|    **Version**            |    **Date**                |    **Author**            |    **Amendments**                                                                                                    |    **Status**    |
|:--------------------------|:---------------------------|:-------------------------|:---------------------------------------------------------------------------------------------------------------------|:-----------------|
|                  0.1.0    |           07-02-2023       |    Skyler Vermeer        |    Layout                                                                                                            |    Layout        |
|                    1.0.0  |                06-04-2023  |    Skyler Vermeer        |    Inputted actual learning outcomes + started filling in current information                                        |    First Draft   |
|                    1.1.0  |                13-04-2023  |    Skyler Vermeer        |    Reformatting layout according to comeback day overview + adding research questions, approach and start of Process |    First Draft   |  

### Communication

|  **Version**  |  **Date**  |  **To**  |
|:--------------|:-----------|:---------|
|  XXX          |  xxxx      |  xxxx    |
|  XXX          |  xxxx      |  xxxx    |  
<div style="page-break-after: always;"></div>

## Introduction
xxxx

## Table of Contents
- [Document History](#document-history)
  - [Version](#version)
  - [Communication](#communication)
- [Table of Contents](#table-of-contents)
- [The Assignment](#the-assignment)
  - [Context](#context)
    - [Problem](#problem)
    - [Opportunity](#opportunity)
    - [Goals](#goals)
- [Process and Results](#process-and-results)
  - [Analysis/Problem definition](#analysisproblem-definition)
    - [Project Plan](#project-plan)
  - [Advice](#advise)
    - [Project Plan](#project-plan-1)
    - [Research Document](#research-document)
  - [Design](#design)
    - [Research Document](#research-document-1)
  - [Realization](#realization)
    - [Research Document](#research-document-2)
  - [Manage & Control](#manage--control)
    - [Research Document](#research-document-3)
- [Proof of Competence](#proof-of-competence)
  - [Thingy](#thingy)
    - [Document](#document)
  - [Thingy](#thingy-1)
	  - [Document](#document-1)
  - [Thingy](#thingy-2)
	  - [Document](#document-2)
  - [Thingy](#thingy-3)
	  - [Document](#document-3)
<div style="page-break-after: always;"></div>

## The Assignment
### Context
The Lectorate ‘High Tech Embedded Software’ from Fontys HBO ICT researches methods and techniques to enhance software development for smart high-tech embedded systems.

The project concerned the Digital Twin Academy, City Digital Twin specifically, which conducts case studies on digital twins to enrich open data of air quality with public area info and other data sources to define useful eco-score. 

#### Problem
One of the biggest problems with the most wicked city problems like air pollution or biodiversity loss is that people are not aware of their seriousness, which is why the City Digital Twin project wants to make this more tangible.

#### Goals
The goal of this project was to make the seriousness of the most wicked city problems more tangible.  The desired product was an application that uses Augmented Reality to visualize location-based data in such a way that it is understandable to the people at that location.

It is possible to make Digital Twins of cities for specific contexts and goals. One of these is informative visualizations, which is what our project consists of. This project should both work towards its goal of making an informative visualization of city problems, as well as be useful for future City Digital Twin projects.

#### Research Questions
For this project, the main research question is:
"How can the severity of city problems in Eindhoven be visualized using AR in a way that is accessible and understandable to the users?"

The key questions that are the foundation of this project in answering the main question are the following:
- Which city problems are being monitored in Eindhoven?
- What impact do these city problems make?
- How can this problem be visualized?
- What is the best Augmented Reality technique for this application?
- What data is available regarding this problem?
- How can AI be used to make predictions using this data?
- What devices will the AR application be available on?
- How can it be verified whether users understand the severity of the city problem?

#### Approach
The approach for this project was a combination of Agile and Waterfall. At the start, the research would be done using a waterfall approach, and once the iterative stage in the project was reached this waterfall approach would be combined with an Agile approach. It would consist of a total of five phases:

Phase 1 - Problem Definition  
Phase 2 - Research and Experiment   
Phase 3 - Component Development  
Phase 4 - Combined Development  
Phase 5 - Finalize

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

## Conclusions
What did I conclude

## Recommendations
What do I recommend

## Personal Reflection

#### Professional Duties
You carry out the professional duties on a junior bachelor level resulting in professional products in line with the IT-area you are working in.

{All or a subset of the activities Analysis, Design, Realize, Advise, Manage&Control}
{Show professional products + ex. IT festival}
##### Document
xxxx


#### Situation-orientation
You apply your previously acquired knowledge and skills in an authentic context to deliver relevant results for the project and company.

{You work in a methodological and structured way, adapted to the processes and way of working of the company}
{you are embedded in an IT environment and work on a given IT problem with multiple stakeholders}
{Link to previous achievements or smth / why are these results relevant to the company}
##### Document
xxxx

#### Future-Oriented Organisation
You explore the organisational context of your project, make business, sustainable and ethical considerations and manage all aspects of the execution of the project.

{Project plan, decision making, managing}
##### Document
xxxx

#### Investigative Problem Solving
You take a critical look at your project from different perspectives, identify problems, find an effective approach and arrive at appropriate solutions.

{Show where I had problems, how I solved them, critical look etc}
##### Document
xxxx

#### Personal Leadership
You are entrepreneurial around your projects and personal development, you pay attention to your own learning ability and keep in mind what kind of IT professional and/or what type of positions you aspire to.

{Feedback, Knowledge sharing Sessions, other presentations etc}
##### Document
xxxx

#### Targeted Interaction
You determine which partners play a role in your project, collaborate constructively with them and communicate appropriately to achieve the desired impact.

{Who did I talk to and how}
##### Document
xxxx