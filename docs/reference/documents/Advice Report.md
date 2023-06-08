---
layout: page
title: Advice report
permalink: /documents/advice-report
parent: Documents
grand_parent: Reference
nav_order: 6
---
# Digital Twin Cities AR Zoom Lens - Advice Report

**_Fontys Lectorate High Tech Embedded Systems Eindhoven_**  
_Eindhoven_  

| Date    | 23-05-2023     |
|---------|----------------|
| Version | 0.1.0          |
| Status  | status         |
| Author  | Skyler Vermeer |  

## Document History

### Version

| **Version** | **Date**   | **Author**     | **Amendments**                                  | **Status**     |
|:------------|:-----------|:---------------|:------------------------------------------------|:---------------|
| 0.1.0       | 23-05-2023 | Skyler Vermeer | Layout + basics of what I want to discuss there | Draft          |
| 1.0.0       | 24-05-2023 | Skyler Vermeer | 'formal' description of advice                  | Finished Draft |  
| 1.1.0       | 25-05-2023 | Skyler Vermeer | Spell check and formatting                      | Finished Draft |  


### **Communication**

| **Version** | **Date**   | **To**       |
|:------------|:-----------|:-------------|
| 1.0.0       | 24-05-2023 | Simona Orzan |
| XXX         | xxxx       | xxxx         |  

## Introduction

In this document, we will answer the question "What steps need to be taken if the project is continued?".

The goal of this document is to highlight the areas that we have found need to be improved.

This advice is based on feedback from stakeholders, users and functionalities that fell outside of the project to
research properly. It also contains advice on which domains or specializations are best fit for this project.

## Table of Contents

- [1. Research](#1-research)
  - [1.1 Trustworhiness](#11-trustworthiness)
  - [1.2 Branding](#12-branding)
- [2. Designs](#2-designs)
  - [2.1 Viewport](#21-viewport)
  - [2.2 User Experience](#22-user-experience)
- [3. Functionality](#3-functionality)
  - [3.1 Specificity](#31-specificity)
  - [3.2 Speed](#32-speed)
  - [3.3 Variables](#33-variables)
- [Conclusion](#conclusion)

## 1. Research

There are several things that we weren't able to research within the scope of the project. This chapter will detail
research-focused improvements to the project.

### 1.1 Trustworthiness

One of the features that we weren't able to sufficiently research to integrate within the scope of the project was
the accuracy of the visualisation. There are so many factors that have an impact on the visualisation, that it isn't
clear to people how 'real' what they are seeing is. We know that the closer to the sensor, the better and the more time
since the last data that was communicated the less accurate the results, and that we are estimating the weight of the
fine dust. However, to communicate these informed assumptions and decisions to the user the design incorporates a '
measurement' of accuracy or trustworthiness. To do this feature justice, more research needs to be done on the impact of
all of these variables and how to communicate this to the user.

This is a combination of data analysis and user design and interaction. This could be combined with research on how to
improve trustworthiness, for example by adding locations. This is probably best fit for either an internship or a
multidisciplinary team with someone focusing on the data and someone focusing on the users or visualizations.

### 1.2 Branding

To allow the application to create impact, by sharing it with the user, it first needs to be developed further so it is
less of a prototype/proof of concept and more of an actual application or project. This branding will probably zone in
more on the use cases of the application. Is it something you only visit once or is it similar to buienradar. Who is the
application targeting? What will the role of the application be, informing the user that fine dust exists, instilling
fear or warning the user of how bad the situation is or can be, or calling the user to action advising them on how to
adjust their behaviour? How will it be shared with the user, posters, Instagram, or news outlets? If users don't know it
exists they won't be able to use it.

This is mainly a project for media students. If you just want them to research, it is probably best fit for earlier
semesters of media design. If it is combined with the advice for designs from Chapter 2, it is also fit for the later
semesters.

## 2. Designs

We were able to iterate somewhat on the designs, however, creating a professional-looking application supported by
knowledge of User Experience and User Interaction was not possible within the scope of the project based on our existing
skillset. The following topics will need several iterations in designs and testing but are important to do before
releasing the application to the user.

### 2.1 Viewport

Currently, the view of the user is a 'screen' with models of different sizes and colours randomly positioned on it. The
screen is bigger than the phone screen of the user but the edges can be found by tilting the device. Further testing
needs to be done on how to improve the location of the different models. For this you can look at the models themselves, are
the sizes and colours acceptable or can they be improved. You can also look at the location, how far away from the user
does it need to be, should the user be able to adjust the distance themselves, for example by walking away? Should it be
static or should it contain some sort of animation? If a 'slice' such as the current prototype is kept, it might be
worth fixing that the plane visual can only be shown 'behind' the user. If you try to place it in front of the user, it
disappears, this is not the case with the particle models.

This is mainly something that is based on designs and user testing. This can be done by people who want to learn more
about Augmented Reality or people interested in User Interaction and User Experience. Designs aren't enough though, as
it needs to be tested so they need to be able to code well enough to integrate it into the current high-fidelity
prototype.

### 2.2 User Experience

Users need more guidance on what they are seeing. Essentially, the user experience throughout the application needs to
be improved. The best example is to create a kind of tutorial explaining how to use the application or where they can
find certain information. This can be done within the Augmented Reality, or for example in the info tab. Testing needs
to be done to see whether also adding values in combination with the visualisation would help. It needs to be
communicated to the user that what you are seeing is a zoomed-in version of how many particles there would be in 1 dm<sup>3</sup>,
but how can we communicate this to the user, how small it is what they are seeing?

This is specifically for people who are working on User Experience but can be done through multiple areas. This could
be done through improving designs or visualisations, better visuals of the data or just in general introducing a tutorial. So
it can be done through a focus on visuals/designs, data or (software) functionality.

## 3. Functionality

This chapter details certain features that aren't enough to be a project on its own, but are advised to improve the
functionality of the project.

### 3.1 Specificity

The distance between the user and the sensor can be quite large. By integrating more data sources the specificity of the
visualization can be improved. The advice is to start by integrating [Sensor Community](https://sensor.community/en/) and research other data
sources that can be added. Additionally, the weather data might also be able to be improved. Currently, all data uses the
KNMI weather data for Eindhoven until May 15th, and Eindhoven in the WeatherAPI afterwards. There might be data for
closer weather stations, especially for the sensor outside the city but inside the municipality of Eindhoven. Even
within WeatherAPI coordinates can be added.

This concerns the integration of data sources into the application and is most likely best fit for a Software student or
anyone who wants to learn to work with APIs. If the model is going to be improved based on new weather data it could
also be used for Artificial Intelligence students.

### 3.2 Speed

There are several aspects in which the application is too slow. The request for the closest location can take more than
a minute, and even then might time out. It is currently using a KNearestNeighbour model so it is scalable when more
locations get added and might be combined with the two closest locations. A way needs to be found to improve the process
of requesting the location of the user, saving it and requesting the relevant data from the API.

Another aspect of how the speed might be improved is the updating of the data. Currently, the data is automatically
updated whenever a user requests that location. If a location isn't visited in a while, this might take a while. So
automatically updating every location, for example, during the night when the use of the application is expected to be
low, might help with this. Additionally, with the way the docker container is currently set up, it is saved to the csv,
but this save is overwritten when you push a new image. A better way, for example by storing it outside the image, needs
to be found to improve this.

This is most likely a project for Software.

### 3.3 Variables

The original design included the option to change the weather variables. This way the user got more autonomy and could
see how different variables changed the amount and behaviour of the particles. The more the user can interact with the
visualisation, the better they understand why time, rain, temperature and wind affect the data that is presented.

You could add this functionality by adding an animation and having the weather variables influence it, or by adjusting
the model or creating a new model to use the weather variables for a past, present or future prediction.

The ability to specify better what time or location the user wants to view, from the past, present or future might also
be a good improvement but this needs to be verified.

This project is best fit for software, user interaction or artificial intelligence students, depending on which aspect
they wish to focus on.

## Conclusion

The project itself is a verified project to work, but still needs some adjustments to be made publicly available. In
the research, design and functionalities several aspects need to be improved. It appears that Media
students are most needed, but Software and Artificial Intelligence students also have enough opportunities within the
project. A multidisciplinary team could elevate the project to a higher level, improving and testing multiple
aspects simultaneously.
