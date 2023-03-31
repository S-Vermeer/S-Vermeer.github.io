# City Digital Twin AR Zoom Lens - Project Plan

**_Fontys Lectorate High Tech Embedded Systems Eindhoven_**  
_Eindhoven_  

| Date    | March 3rd 2023 |
|---------|-------------------|
| Version |             2.3.2 |
| Status  | Second Draft            |
| Author  | Skyler Vermeer    |  

<div style="page-break-after: always;"></div>

### Version

|          **Version**                   |          **Date**                                    |          **Author**                            |          **Amendments**                                                                                                                                                                                      |          **Status**                                   |
|:---------------------------------------|:-----------------------------------------------------|:-----------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------|
|                         1.0.0          |          February 6th 2023                           |          Skyler Vermeer                        |          Putting info project proposal into template project plan                                                                                                                                            |          Draft                                        |
|          1.1.0                         |          February 9th 2023                           |          Skyler Vermeer                        |          Added information missing from project proposal                                                                                                                                                     |          Initial feedback hand in.                    |
|                            2.0.0       |       February 13th 2023                             |       Skyler Vermeer                           |       Added introduction                                                                                                                                                                                     |       Extended draft                                  |
|                             2.1.0      |      February 14th                                   |      Skyler Vermeer                            |      Updated Research question based on feedback                                                                                                                                                             |      Second Draft                                     |
|                              2.2.0     |     February 15th 2023                               |     Skyler Vermeer                             |     Start working on breakdown/approach                                                                                                                                                                      |     Second Draft                                      |
|                               2.3.0    |    February 17th 2023                                |    Skyler Vermeer                              |    Added basic info test approach and configuration management                                                                                                                                               |   Second Draft                                        |
|                                2.3.1   |   February 27th 2023                                 |   Skyler Vermeer                               |   Added Software Architecture Document to deliverables.                                                                                                                                                      |   Second Draft                                        |
|                                 2.3.2  |  March 3rd 2023                                      |  Skyler Vermeer                                |  Processed Feedback Simona (1.3 last requirement, City Digital Twin, User experience subquestions and adjusted main question, small adjustments 3.3 and 3.4)                                                 |  Second Draft                                         |  

### Communication

|  **Version**  |  **Date**  |  **To**  |
|:--------------|:-----------|:---------|
|  1.1.0          |  February 9th 2023      |  Frank Schürgers    |
|  2.3.0          |  February 17th 2023      |  Simona Orzan    |
|  2.3.2          |  March 3rd 2023      |  Frank Schürgers    |

<div style="page-break-after: always;"></div>

## Abstract
In this document we will detail the terms and conditions on which this project is based. 

The goal of this document is to ensure at the end of the internship period there are no surprises or disagreements about the final result.

To do this, we will detail the assignment so it is agreed upon by all parties, the approach that will be taken while executing the project, and the agreements made about things such as communication.

<div style="page-break-after: always;"></div>

## Table of Contents
- [Version](#version)
- [Communication](#communication)
- [Introduction](#introduction)
- [Table of Contents](#table-of-contents)
- [1. Project Assignment](#1project-assignment)
  - [1.1 Context](#11context)
  - [1.2 Goal of the project](#12-goal-of-the-project)
  - [1.3 The assignment](#13the-assignment)
  - [1.4 Scope](#14scope)
  - [1.5 Conditions](#15conditions)
  - [1.6 Finished products](#16finished-products)
  - [1.7 Research questions](#17research-questions)
- [2. Approach and Planning](#2-approach-and-planning)
  - [2.1 Approach](#21approach)
    - [2.1.1 Test approach](#211test-approach)
  - [2.2 Research methods](#22research-methods)
  - [2.3 Breakdown of the project](#23breakdown-of-the-project)
  - [2.4 Time plan](#24time-plan)
- [3. Project Organization](#3project-organization)
  - [3.1 Team members](#31-team-members)
  - [3.2 Communication](#32communication)
  - [3.3 Test environment](#33test-environment)
  - [3.4 Configuration management](#34configuration-management)
- [4. Other](#4-other)
<div style="page-break-after: always;"></div>

## 1. Project Assignment

### 1.1  Context

The Lectorate ‘High Tech Embedded Software’ from Fontys HBO ICT researches methods and techniques to enhance software development for smart high-tech embedded systems.

The project will be concerning the Digital Twin Academy. The work of Fontys focuses on case studies on digital twins (WP4). Case studies will be conducted on:

- Smart Industry Twin - to enable small and medium enterprises to work on their digitization.
- City Digital Twin - to enrich open data (air quality) with public area info and other data sources, to define useful eco-score.
- Robot4Care Twin - to improve care/hospital environments with robots and IoT.
- Virtual/animal Twin - to speed up footage to learn agro tools working in greenhouses with vegetables and conduct continuous experiments with animals.

Where in the case of this internship, it is in the context of the City Digital Twin.

One of the biggest problems with the most wicked city problems like air pollution or biodiversity loss is that people are not aware of their seriousness, which is why the City Digital Twin project wants to make this more tangible.

Currently, there have been several student projects that have focused on air quality. They are a lot of separate projects that are currently being gathered and an inventory is being created for future use. So some relevant aspects of this project have already been researched before, however, there is still enough unknowns that have to be researched.

The current known stakeholders are:  

- High Tech Embedded Systems as they are using the project for their Digital Twin Academy.
- Simona Orzan, as she is the one leading the City Digital Twin  Research

### 1.2 Goal of the project

The goal of this project is to make the seriousness of the most wicked city problems more tangible.  The desired product is an application that uses Augmented Reality to visualize location-based data in such a way that it is understandable to the people at that location.

It is possible to make Digital Twins of cities for specific contexts and goals. One of these is informative visualizations, which is what our project consists of. In the past, other small practical research 'nuggets' have already contributed to the City Digital Twin project. This project should both work towards its goal of making an informative visualization of city problems, as well as be useful for future City Digital Twin projects.


### 1.3 The assignment

Make the seriousness of city problems more tangible with an AR app that ‘zooms in’ on the environment (air, soil) to a microscopic level. Based on the real-time location-based data, generate an AR picture of how the environment might look like if seen under a powerful microscope. Represent accurately the density and size of each particle type.

Though the minimum requirements still need to be properly formatted, essentially they are the following:
- It needs to contain a data stream that is visualized in Augmented Reality.
- The data needs to be location-based, based on coordinates.
- There needs to be an application that is visible on the phone.
- There is no prior domain knowledge required, that is the visualized information should be understandable for any user.

### 1.4 Scope

**The project includes:**
- Augmented Reality application that uses location-based data related to a single city problem.
- Artificial Intelligence prediction of the progression of the city problem.
- Publically accessible, reproducible code and research.
- Continuous Integration and Continous Deployment

**The project does not include:**
- Management of the application after the internship.
- Protection against Cyber attacks.
- Data outside Eindhoven.

### 1.5 Conditions

The only preconditions are the use of Augmented Reality and location-based Eindhoven data.

### 1.6 Finished products

End product: AR zoom lens application – Augmented Reality, Camera, (Real-time) Data – Application that uses AR to zoom in on the problems of the city.

Intermediary Products / Proof of Concepts:
- Augmented Reality Test – Phone camera, Augmented Reality – POC of how augmented reality could work, without real-time data
- Real-time data test – Location Tracker, API calls – POC of obtaining real-time data and visualizing it
- Artificial intelligence Predictions – Artificial Intelligence, API calls – POC of what types of predictions can be made based on current and past data.

Final additional deliverables:
- Project Plan - What you are currently reading.
- Research document – Answers the Main and Sub Research Questions
- Exploratory Data Analysis - What does the data look like
- Feedback Report – Depending on the stage of the project, through various testing methods (AB-testing, wireframes, actual testing) get user feedback on whether the product is working as intended and how to improve it.
- Version Controlled Code - The code behind the application, including a ReadMe so anyone can continue to develop the project or work on a different implementation based on it.
- Software Architecture Documentation - The additional choices behind the software architecture documented, such as a C4 diagram and the CI/CD pipeline.
- Portfolio - How I have progressed in the project.

### 1.7 Research questions

These are the most important research questions to be answered. The rest of the project will be supported with more sub-questions, however, these are the key research questions. In this project, I will research the city problems, and keep them in the back of my mind while answering the research question, but focus on a single one that I will choose after my initial research is done.

Main: How can the severity of city problems in Eindhoven be visualized using AR in a way that is accessible and understandable to the users?

The subquestions can be divided into a few sections:

**City Problems**
- Which city problems are being monitored in Eindhoven?
- What impact do these city problems make?

After this section, I will make a choice on which problem I will focus on, and the rest of the questions will be answered based on this problem.

**Visualization**
- How can this problem be visualized?
- What is the best Augmented Reality technique for this application?

**Artificial Intelligence**
- What data is available regarding this problem?
- How can AI be used to make predictions using this data?

**User Experience**
- What devices will the AR application be available on?
- How can it be verified whether users understand the severity of the city problem?
<div style="page-break-after: always;"></div>

## 2. Approach and Planning

### 2.1 Approach
We will be using a combination of Agile and Waterfall. At the start we will be using a waterfall approach, and once we reach the iterative stage in our project we will combine this with an Agile approach.

This assignment assumes that there are multiple city problems in Eindhoven, and wants to make one of them more tangible. The problem definition phase will be figuring out which problems are possible to research during this internship, choosing which one to continue with and researching what the exact problem with comprehending the severity is for our target audience of the 'common person in Eindhoven'.

#### 2.1.1 Test approach

There is nothing decided yet, however, my company coach has offered to do code reviews for me.

### 2.2 Research methods

**Main: How can the severity of city problems in Eindhoven be visualized using AR?**  
The final product will be an AR application with location-based data. For this, I will need to design and develop an application based on research. Though the specific questions that will be answered are not yet known, the following research methods will most likely be used:  

- *Document Analysis* - The City Digital Twin project already has done multiple pieces of research. Part of my research will be analysing these documents, what has already been researched and achieved and the insights gained from there.
- *Observation* - To ensure the severity is tangible, I need to see whether other people think and behave as expected. Some form of observation will probably be included in testing.
- *Peer and/or Product Review* - To see how the product can be improved, a peer and/or product review can be used as an outsider's perspective.

**Which city problems are being monitored in Eindhoven?**  

The domain of this assignment concerns city problems in Eindhoven. To see which specific problems these are and what they mean, I will do a *Literature Study* and *Problem Analysis*. This includes studying the data I have access to, what they describe and whether they relate to the context of the assignment.

**What impact do these city problems make?**  

Part of the previously named *Problem Analysis* will also relate to this question, as what impact they make and how it can be made tangible is part of the assignment so defining the exact problem here is crucial. This will probably need to be combined with more *Literature Study*.

Ending this phase of research we will choose a problem to continue researching which will be done using *SWOT analysis*. Based on this we will make a *Requirements Prioritization* so we can adjust our planning accordingly.

**How can this problem be visualized?**  

Once the city problem is determined, how it can be visualized is the next most important thing, which is also regarding the *Problem Analysis* as defining the exact problem here is crucial. In addition to this, some type of research through design will be done, however, the method for this is undetermined so far.

**What is the best Augmented Reality technique for this application?**  

Augmented Reality is a requirement from the stakeholder, however for further development and to be able to design and develop accordingly, a *Literature Study* needs to be done as to the different techniques of AR and how they fit in this context. This can be substantiated with some form of user testing such as *Usability Testing*.

**What data is available regarding this problem?**  

Eindhoven location-based data visualization is a requirement from the stakeholder, however, to know what we are visualizing we need to do some *Data Analytics*.

**How can AI be used to make predictions using this data?**  

As this project is done in the context of an AI internship, artificial intelligence is also an important aspect. To see which AI will be created, some *Data Analytics* is needed and based on that a Prototype will be created. After testing that prototype we can see whether the predictions have the desired effect.

**What devices will the AR application be available on?**  

In our requirements we have that it should be available on a phone. This will detail which devices will be supported and why. It will also detail how these decisions were made.

**How can it be verified whether users understand the severity of the city problem?**  
To verify whether the intended users can understand our product, we will be using a mix of *Observation*, to see what our users behave like, and various user testing methods to see how the user responds to the application.

### 2.3 Breakdown of the project

Phase 1 - Problem Definition  
- Project Plan
- City Problems Research and Choice

Phase 2 - Research and Experiment   
- Research Augmented Reality
- POC using Augmented reality without our data
- Design first iteration AR
- Explore Possible models

--- 
*Verify with Stakeholder specifically am I okay to continue*

---
Phase 3 - Component Development  
- Design, develop, and feedback for visualization iteratively
- Improve and Evaluate model iteratively

Phase 4 - Combined Development  
- Design, develop, and feedback including AI model

Phase 5 - Finalize
- Process for deployment
- Advise future students
- Finalize Portfolio

[//]: # (Works for gitlab. For local reference use ![waterfall](Waterfall-draft1.png))
![Waterfall Draft](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/Waterfall-draft1.png)

### 2.4 Time plan

|   **Phasing**                |   **Start**           |   **Ready**    |
|:-----------------------------|:----------------------|:---------------|
|        1 Problem Definition  |   Week 1              |   Week 3       |
|   2 Research and Experiment  |   Week 3              |   Week 6       |
|     3 Component Development  |   Week 6              |   Week 14      |
|      4 Combined Development  |  Week 14              |  Week 16       |
|                  5 Finalize  |  Week 16              |  Week 18       |     
<div style="page-break-after: always;"></div>

These phases overlap with a week, as they aren't necessarily hard rules, but if I am within these boundaries I know I am on schedule.

## 3. Project Organization

### 3.1 Team members

This project will mostly be an individual project, with support from my two coaches. One representing FHICT and one representing Fontys Lecorate HTES. I can consult with those associated with the Lectorate as needed, however, this is on an as-needed basis so it is not yet determined who these people will be. I will also have a workplace with the other Lectorate interns whom I can consult with as needed, however, they too are not mandatory in the organization.

|   **Name + Phone + E-mail**                                      |   **Role/Tasks**                             |   **Availability**                                                                                                                                                                      |
|:-----------------------------------------------------------------|:---------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Skyler Vermeer - 0652132782 - s.vermeer@student.fontys.nl      |   Project Researcher & Developer             |                                                                                                                                       5 days a week, usually from 8:30 - 16:00 / 17:00. |
|   Simona Orzan - 0885079151 - s.orzan@fontys.nl                  |   Product Owner, Internship Coach            | Available through teams at any time, set meeting once a week, optional meeting during the week is possible on request. Thursdays always on location, other days not always on location. |
|  Frank Schürgers - 0885084386 - f.schurgers@fontys.nl            |  FHICT Coach                                 |  Available at TQ 4 days a week, on Tuesdays online as they are in a different location.                                                                                                 |
| Dennis Kirsch - 0885089149 - d.kirsch@fontys.nl                  | Lectorate Coach                              | At least on Tuesday, Wednesday and Friday.                                                                                                                                              |
| Teague Murray Marshall - 0885087687 - t.murraymarshall@fontys.nl | Lectorate Coach                              | At least on Tuesday, Wednesday and Friday.                                                                                                                                              |
| Bart van Gennip - 0885078966 - b.vangennip@fontys.nl             | Lectorate Coach                              | At least on Tuesday, Wednesday and Friday.                                                                                                                                              |  

### 3.2 Communication
Weekly meetings on Thursday with Simona, with the possibility of more meetings during the week as needed. 

Frank is available on request but requires a first draft of the portfolio on the 15th of May. The project plan should be approved and finalized in week 5, and the research document in week 8.


### 3.3 Test environment

CI/CD will be used when necessary, with whatever has the least amount of effort. This means probably using Jenkins and sonarcube as those are services I have used before.

The test environment will most likely be a few basic unit tests and mostly user testing as that is what we want to research. It will most likely be a combination of peer and product reviews and other testing methods. We will also need to test and tune and evaluate our machine learning models, to ensure they are functioning as intended.

### 3.4 Configuration management

My GIT repository is set up with three directories: Code, Data Analysis and Research. In research all my documents and research and things like that are placed. In Code is the physical app code, in data analysis will be the data and the AI development until it is combined with the app. Seeing as I am the only person working on the app, branches aren't specifically necessary. I will be using them for experimental implementations that I want to be able to roll back. I can also use it to ensure I always have a functioning product to demo.

This GIT repository has both Simona and Frank as reporters. If a different roll or a different person is needed, they can always be added or updated, however for now this allows easy tracking of my progress even if I haven't handed in something in canvas.
<div style="page-break-after: always;"></div>

## 4. Other

_<< Describe here everything that is relevant but that you cannot put elsewhere in the document.>>_
