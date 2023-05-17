# City Digital Twin AR Zoom Lens - Feedback Report
**_Fontys Lectorate High Tech Embedded Systems Eindhoven_**
_Eindhoven_

|  Date     |         30-03-2023  |
|:----------|:--------------------|
|  Version  |              1.0.0  |
|  Status   | Draft               |
|  Author   |  Skyler Vermeer     |  

<div style="page-break-after: always;"></div>

## Document History
### Version

|    **Version**             |    **Date**                       |    **Author**                      |    **Amendments**                                                             |    **Status**             |
|:---------------------------|:----------------------------------|:-----------------------------------|:------------------------------------------------------------------------------|:--------------------------|
|                   0.1.0    |                  07-02-2023       |    Skyler Vermeer                  |    Layout                                                                     |    Layout Draft           |
|    0.2.0                   |    16-02-2023                     |    Skyler Vermeer                  |    Introduction added                                                         |    Layout Draft           |
|                      1.0.0 |                        30-03-2023 | Skyler Vermeer                     | Added Prep + Feedback Clickable Prototype                                     | Draft                     |  

### **Communication**

|  **Version**  |  **Date**  |  **To**  |
|:--------------|:-----------|:---------|
|  XXX          |  xxxx      |  xxxx    |
|  XXX          |  xxxx      |  xxxx    |  
<div style="page-break-after: always;"></div>

## Introduction
In this document we will answer the question "How can the Augmented Reality application be improved upon?".

The goal of this document is to get outsider feedback to improve our application.

To do this, we will test our POC's at various different stages with various different testing methods and various different people and document the testing and feedback and improvements based on it. This way the parts that we can improve within our scope will be improved and the parts outside our scope will be noted down for advice.
<div style="page-break-after: always;"></div>

## Table of Contents
(Ctrl + alt + T)
<div style="page-break-after: always;"></div>

## 1. Clickable Prototype / Wireframe 
### Introduction
To validate our designs make sense to the possible users as well as see in which ways they can be improved, we made a clickable prototype. However the goal of this was not to validate the user behaviour but to ensure the designs and concept of the application are clear. That is why it would be more a 'clickable wireframe'. The designs in question are:
<!-- works in gitlab. For local referencing use:
![[initial-designs-app1.png]]
![[initial-designs-app2.png]]
-->
![initial designs app1](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/initial-designs-app1.png)
![initial designs app2](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/initial-designs-app2.png)

### Preparation
#### Expo / Survey
On March 27th there was an interactive poster market where students could use the visitors to verify their concepts and projects. In preparation for this, we made a survey to test our clickable prototype.  

The goal of our survey is to get an overview of our testers so we could relate them to our personas, see what their current knowledge of fine dust is, and get feedback on both the concept and designs.

The questions prepared in a printed survey were:

**General**  
What is your age:  
What is your gender:  
What is your highest form of education:  
Are you busy or do you have a lot of free time?: _Busy_ |----------------------------------------| _Free time_  
Do you stay in one place or move around a lot: _Stationary_ |----------------------------------------| _Moving_  

**Domain**  
Do you know what fine dust is?:  
Do you know why it is being monitored?:  
Do you know what the main health concerns are?:  

**Concept**  
The A(I)R Zoom lens is an application meant to make fine dust more tangible, to make people aware it exists, is a problem, and how big of a problem it is at that moment for them.  
Do you understand the concept?:  
Would you use the application based on this concept?:  
For what goal would you use this application? You are allowed to refer to the personas for examples:  
Do you have any questions regarding the concept?:  
Do you have any tips/feedback based on solely this concept?:  

**Design**  
There is a clickable prototype available with designs for the application. You can click through the application to see what it might look like, and if you press the ‘AR’ Navigation button multiple times you can cycle through multiple different designs.  
What is the first interesting thing you notice about the designs?:  
What is the first point of annoyance about the designs?:  
Please describe the AR design you prefer:  
Do you have any feedback about the designs?  

**Additional Feedback:**  

#### Through other eyes
The goal of this was to validate our clickable wireframes through the eyes of someone else as they might see something we did not notice. In this case we had similar aspects as the survey, except we focused on validating the concept and  design instead of also testing the base knowledge of the testers and relating them to the personas.

For this we explained the concept, walked them through the prototype to show the different designs for them to provide feedback on and allowed them to comment on it.

### Feedback
#### Expo
Unfortunately the poster market had very limited amounts of visitors, of which most were fellow students or teachers who visited other stands. After conferring with my stakeholder and collegues we decided to ensure that we could validate the designs at least by talking to some Media Design students later that week.

#### Media Design Students
##### Group 1 - 3 Guys
The guys had some feedback. They also usually agreed with each others feedback, with the exception of the selected tab not being clear. They also mentioned that they liked the concept and would like it on their own phone. Their feedback was the following:

**General**
- The selected tab is not clear (other group members recognized that there was a different color for the selected tab)
- You should add icons to the navigation bar, so a house for the home button, some type of virtual reality icon for the AR tab etc.

**Home**
- The moving gif at the home page is nice

**AR tab**
- Make the sliders the same (alternatively, figma has iOS sliders. We are not focusing on iOS). The difference in sliders between the environment variables and time sliders is confusing.
- Label the icons. It is not clear what each icon means right now.
- The color scale for confidence is nice (it was not intuitive right now as it didn't change, but they agreed with the plan after commenting on the color of the confidence indicator)
- All the variables on one page is the preferred design but put all the variables together, preferrably at the bottom. This makes it more ergonomical so the user doesn't constantly have to adjust how they select the sliders.
- The zoom in effect is nice (in the circle). It aids the concept.

**Info tab**
- Make the graph bigger, especially the legend. On pc it is already small, on a phone it is even smaller.
- Make the text bigger, it will be harder to read on the phone.
- Look at the figma community how they do graphs.

##### Group 2 - 6 Girls
The girls had some feedback and generally also mentioned that they agreed with previously made feedback. They also mentioned that they liked the concept and would like it on their own phone. Their feedback was the following.

**General**
- The title, A(I)R Zoom lens is nice
- They like the minimalistic simplicity of the application.
- The green theme is nice, it fits the sustainability concept

**Home**
- Add icons to the permissions, so you can remove the text.
- Add sliders to the permissions instead of the on/off buttons.

**AR tab**
- Make it more intuitive, for example confidence is not clear, but accuracy would be better. Make the low confidence more prominent, as it is a bad thing so more prominent red. You could also just make the text red so it would fit within the theme.
- They like the full screen AR vision, but with the idea that you could zoom in on the areas by tapping on them and then getting the zoomed in screen. (This functionality is not possible seeing as we work using gps coordinates and we can only ever be as accurate as those gps coordinates)
- Give the user more autonomy by allowing them to edit which sliders they want to see.
- Move the sliders down to make it easier for the user to adjust the sliders.
- You can barely see the smaller particles.

**Info tab**
- Make the text bigger, it is hard to read on phone.
- Make bullet points for info so it is easier to read.
- Make the visual graph more interactive, for example if you select PM10, you get more information about that particle size.

#### Conclusion
Based on this, we can see that the concept itself is good and liked, but there are some adjustments to be made. The main repeating ones are to make the text larger, add icons to text or replace text with icons and add labels to existing icons and moving the sliders to the bottom. Based on that we can make some new designs:
<!-- works in gitlab. for local referencing use:
![[adjusted-designs-app.png]]
-->

![updated designs app](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation/-/raw/main/Copy%20paste%20images/adjusted-designs-app.png)  
In this case, the info screen has a scroll functionality. Text was made bigger, a settings screen was added, icons and labels were added, and the permissions were replaced with icons and sliders. The sliders were all moved to the bottom of the page.

The girls liked the full screen more, but the functionality they would've liked to see, with a zoom in on a location on a tap is not possible as we get the data in the location scope so we cannot be more specific than a GPS coordinate. The confidence indicator needs some additional testing as to what the best design for it is.
<div style="page-break-after: always;"></div>

## 2. Test Method 2
### Intro info

### Tests desc
#### Test 1
xxxx

#### Test 2
xxxx

### Feedback
#### Test 1
xxxx

#### Test 2
xxxx
<div style="page-break-after: always;"></div>

## Conclusion
xxxx
<div style="page-break-after: always;"></div>

## Sources
xxx