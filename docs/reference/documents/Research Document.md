---
layout: page
title: Research Document
permalink: /documents/research-document
parent: Documents
grand_parent: Reference
nav_order: 3
---

# City Digital Twin AR Zoom Lens - Research Document

**_Fontys Lectorate High Tech Embedded Systems Eindhoven_**  
_Eindhoven_

| Date    | 31-03-2023     |
|:--------|:---------------|
| Version | 2.1.0          |
| Status  | Second Draft   |
| Author  | Skyler Vermeer |     

| **Version** | **Date**   | **Author**     | **Amendments**                                                                                          | **Status**   |
|:------------|:-----------|:---------------|:--------------------------------------------------------------------------------------------------------|:-------------|
| 0.1.0       | 8-2-2023   | Skyler Vermeer | Layout                                                                                                  | Layout Draft |
| 1.0.0       | 10-2-2023  | Skyler Vermeer | Added Research Question 1                                                                               | Draft        |
| 1.1.0       | 13-2-2023  | Skyler Vermeer | Added Research Question 2                                                                               | Draft        |
| 1.2.0       | 16-2-2023  | Skyler Vermeer | Add introductions sections, Spelling + Grammar Check + Chosen domain (conclusion)                       | Draft        |
| 1.2.1       | 3-3-2023   | Skyler Vermeer | Process feedback Simona (1.2 structure + change sources to references + added layout for new questions) | Draft        |
| 2.0.0       | 6-3-2023   | Skyler Vermeer | AR techniques research                                                                                  | Second Draft |
| 2.0.1       | 7-3-2023   | Skyler Vermeer | AR frameworks research                                                                                  | Second Draft |
| 2.1.0       | 16-3-2023  | Skyler Vermeer | Incorporate Expert Interview + adding more structure with headings and such                             | Second Draft |
| 2.2.0       | 31-3-2023  | Skyler Vermeer | Incorporate designs                                                                                     | Second Draft |
| 3.0.0       | 20-04-2023 | Skyler Vermeer | Incorporate POC's in AR research                                                                        | Third Draft  |  

### **Communication**

| **Version** | **Date**  | **To**          |
|:------------|:----------|:----------------|
| 1.2.0       | 17-2-2023 | Simona Orzan    |
| 1.2.1       | 3-3-2023  | Frank Schürgers |
| 2.1.0       | 5-4-2023  | Simona Orzan    |

<div style="page-break-after: always;"></div>  

## Abstract

In this document, we will answer the question "How can the severity of city problems in Eindhoven be visualized using
Augmented Reality?".  
The goal of this document is to create an overview of the research done, with sources, summaries and conclusions. In
case more detailed answers are in another document, the relevant documents will be referenced.    
To do this, we will use the research questions as defined in the project plan and answer them using the documents that
our research consisted of.
<div style="page-break-after: always;"></div>  

## Table of Contents

- [Document History](#document-history)
    - [Version](#version)
    - [Communication](#communication)
- [Introduction](#introduction)
- [Table of Contents](#table-of-contents)
- [1. City Problems](#1-city-problems)
    - [1.1 Which city problems are being monitored in Eindhoven?](#11-which-city-problems-are-being-monitored-in-eindhoven)
    - [1.2 Which impact do these city problems make?](#12-what-impact-do-these-city-problems-make)
    - [Conclusion - Which city problem will we be focussing on?](#conclusion---which-city-problem-will-we-be-focussing-on)
- [2. Visualization](#2-visualization)
    - [2.1 How can this problem be visualized?](#21-how-can-this-problem-be-visualized)
    - [2.2 What are the benefits and deficits of using Augmented Reality for this visualization](#22-what-are-the-benefits-and-deficits-of-using-augmented-reality-for-this-visualization)
- [3. Artificial Intelligence](#3-artificial-intelligence)
    - [3.1 What data is available regarding this problem](#31-what-data-is-available-regarding-this-problem)
    - [3.2 How can AI be used to make predictions using this data?](#32-how-can-ai-be-used-to-make-predictions-using-this-data)
- [Conclusion](#conclusion)

<div style="page-break-after: always;"></div>  

## 1. City problems

The first aspect we need to research is city problems. This is to make sure we pick a problem that is still a problem,
fits with our requirements of Augmented Reality, Location Based Data and Artificial Intelligence, and needs to be made
more tangible.

### 1.1 Which city problems are being monitored in Eindhoven?

The city of Eindhoven is monitoring several problems so they can see the progression and create and evaluate policies
based on it. They also share the data from their sensors publically. This research will focus on the nature and
climate-based problems that can be influenced by humans.

These topics were found on the data portal of Eindhoven, under the topic 'Nature and Environment' (_Explore — Eindhoven
Open Data_, n.d.)[<sup>1</sup>] Based on this list, the following is an
analysis of the causes related to the data that is being monitored.

#### Population Growth

One of the problems that are being monitored is the effects of population growth. The growing urban population is
causing an increase in air temperature, CO<sub>2</sub> and other air pollutant emissions. This has led to a decrease in
stored carbon and is threatening ecosystems and mankind. Because of the higher temperatures, there is an increase in air
pollution due to an increase in ozone formation and as energy demands for cooling increase. (_Impacts of Nature-Based
Solutions on the Urban Atmospheric Environment_, n.d.)[<sup>2</sup>](#^source2-resourcedoc)

#### Urban Growth

Another one of the problems that are being monitored is the effects of urban growth. Due to urban areas expanding,
natural habitat is converted into cities. This leads to a big loss of biodiversity as the number of species is globally
on average 50% lower at urban sites than in intact natural habitats. However, the indirect effect has an even larger
impact. This is due to the increased resources consumed within the city, and the increased pollution released from
cities. It is estimated that the area required to feed the world's cities is 36 times greater than the urban area of
cities (McDonald et al., 2020)[<sup>3</sup>](#^source3-resourcedoc)

Due to the urban growth, there is also more road traffic and construction areas, which are the main sources of particle
matter in metropolitan areas. (Pope et al., 2009) [<sup>4</sup>](#^source4-resourcedoc)

### 1.2 What impact do these city problems make?

Population growth and urban growth each have different consequences, as mentioned in the previous question. This
question aims to explore these consequences and discover which impacts they each make.

#### Increasing Air Temperature

One of the consequences of population growth is that the air temperature is increasing. This is because, with the
increasing population, the demand for resources is also increasing. This leads to higher emissions of greenhouse gasses,
which in return leads to a higher concentration of CO<sub>2</sub> in the air. CO<sub>2</sub> traps heat in the
atmosphere, leading to a heightened air temperature. Though a heightened CO<sub>2</sub> does improve the functions of
some processes in plants, the benefit we gain from this is limited as once the temperature rises too much the heightened
CO<sub>2</sub> can no longer compensate for the diminishing effects caused by temperature-sensitive processes. Wang et
al [<sup>5</sup>](#^source5-resourcedoc) found that after an increase of 2 <sup>o</sup>C the
heightened CO<sub>2</sub> no longer compensated for the heightened temperature. This was based on the temperature in
2018, and the temperatures are estimated to rise between 0.1 <sup>o</sup>C and 0.3 <sup>o</sup>C each decade, and the
rate is increasing (_Chapter 1 — Global Warming of 1.5 oC_,
n.d.) [<sup>6</sup>](#^source6-resourcedoc)

Due to these temperatures rising, the glaciers and ice sheets are melting. The water that comes free from this as well
as the thermal expansion of seawater as it warms is causing the water levels to rise. The global mean sea level has
risen approximately 3.6mm per year from 2006 to 2015. This is an increase from the 1.4mm that was the average of most of
the 20th century. (_Climate Change_, n.d.) [<sup>7</sup>](#^source7-resourcedoc)

#### Increasing Air Pollutant Emissions

Another thing that is increasing in the air, is air pollutant emissions. This is a consequence of both population growth
and urban growth. They are especially noticeable in the increased value of O<sub>3</sub> and NO<sub>2</sub>. Urban
NO<sub>2</sub> daily patterns are noticeably influenced by traffic, with peaks in the morning and late afternoon and a
decrease in concentrations during the weekend. In the winter heating processes are used a lot more often, which can
explain the heightened concentrations even in places that are not or are less influenced by the traffic.

NO<sub>2</sub> is a precursor to O<sub>3</sub>, which is why there is a negative correlation between O<sub>3</sub> and
NO<sub>2</sub>. The increase in solar radiation throughout the day in combination with the presence of other precursors
promotes ozone formation and concentrations increase. During the night, the increase in NO<sub>2</sub> leads to O<sub>
3</sub> consumption. This is why hotter months have higher O<sub>3</sub> concentrations due to the longer and higher
intensity solar radiation. During the weekend, the average O<sub>3</sub> concentrations are higher than during weekdays
due to the lower nitric oxide levels and VOC emissions in the morning, which build up through photochemical reactions. (
_Impacts of Nature-Based Solutions on the Urban Atmospheric Environment_,
n.d.) [<sup>2</sup>](#^source2-resourcedoc)

#### Loss of Biodiversity

One of the other consequences of urban growth is the loss of biodiversity. As a result, the way ecosystems function is
affected in several aspects. It reduces the efficiency by which ecological communities capture biologically essential
resources. These resources are nutrients, water, light and prey. It also reduces the efficiency with which biomass is
produced, as they are dependent on the production of these biologically essential resources. Due to the decrease in
plant litter diversity, the decomposition and recycling of biologically essential nutrients after organisms die are also
affected.

This loss of biodiversity is creating an impact on the stability of ecosystem functions. Different life forms have
different responses or competitive interactions to environmental fluctuations. This can lead to asynchrony which means
the ecosystem becomes unbalanced. Additionally, the number of species needed to sustain a single ecological process is
lower than the number of species needed to sustain multiple processes simultaneously. So even if we were to stabilize a
single process, ecosystems are multifunctional and need more biodiversity to maintain that.

Hooper et al [<sup>8</sup>](#^source8-resourcedoc) revealed that the impacts of species loss on
primary productivity are of comparable magnitude to the impacts of other global change stressors.  
If we try to increase or at least maintain the current biodiversity, this would have a positive impact to combat the
negative effects of both population growth and urban growth. Diversity increases the yield of commercial crops, the
production of fodder and greater stability of fisheries yields. In diverse plant communities, plant pathogens are less
prevalent. It also increases aboveground carbon storage as biomass production is enhanced. Nutrient mineralization and
soil organic matter also increase with plant richness (Cardinale et al.,

2012) [<sup>9</sup>](#^source9-resourcedoc)

#### Increase in Fine Dust Particles

A third consequence of urban growth is an increase in fine dust. Fine dust is a mixture of particles < 10 µm (0.01mm) of
which they are often categorized into the subgroups PM10, PM2.5 and PM1. They are from various sources such as
combustion processes which also produce gaseous substances in addition to particulate matter which combined with
agricultural ammonia emissions contribute to secondary fine dust formation.

The main sources of fine dust formation can be categorized into metropolitan, airport and agricultural. Fine dust
particles come from a variety of sources, natural sources such as sea salt, but also unnatural ones such as wear and
tear and combustion. The natural sources aren't necessarily a problem, we need them as they block parts of the sun and
they reflect the light (Otjes & Close, 2023) [<sup>10</sup>](#^source10-resourcedoc). There
are also varying sizes of fine dust particles, of which PM1 is the most dangerous, excluding Ultra Fine Particles (UFP)
which are so small that measuring them is quite expensive (J.P. Close, personal communication, March 7, 2023).

Fine dust is harmful to the health of people. This is because its chemical and physical properties produce oxidative
stress on mitochondria, DNA and proteins. It can also cause systemic inflammatory reactions. It has been connected with
cardiovascular disease and some cancers (Ritz et al.,2019) [<sup>11</sup>](#^source11-resourcedoc). This effect is
exacerbated when someone smokes, as this gives an immense amount of fine dust particles direct access to the airways.
Not all types of fine dust particles are particularly harmful, for example, sea salt is not as harmful as combustion
fine dust particles. The human body does have some defences to prevent fine dust from entering, but PM1 is so small that
it goes through the external defences of the human body and does not (easily) dissolve in liquid. Moustache hairs block
PM2.5 and PM10, but PM1 still goes through. The same goes for UFP, which can pass through the gaps between the molecules
of which the human body consists. An example of the effects of this would be when a person is pregnant and the fine dust
particles impact the development of the brain of the baby, causing disorders such as ADHD and Dyslexia.

The behaviour of fine dust is dependent on the weather. This is also what makes it so hard to predict, as it is hard to
predict how the weather will have affected the concentration of fine dust particles between now and the moment and
location that is being predicted. Fine dust particles are particularly affected by the wind. It descends and if the wind
is blowing, it disperses and moves with the wind to a different location. This is both a good and a bad thing, as it
means that wind can both remove fine dust particles from our area, as well as bring them to us, which is what happens
when there is a lot of traffic in the morning and the wind blows the particles over the entire city. However, no wind is
even worse, as this means that the particles stay in the air, collecting together until there is a thick layer, often at
breathing level. UFP is an exception in the behaviour of fine dust particles when encountering wind, as they are not
affected by it. Another impactful weather event is inversion. When cold and warm air is pressed together, because of
inversion, the fine dust particles are also pressed together, which often happens at breathing level, making it easier
to breathe in higher concentrations of fine dust. However, precipitation, on the other hand, has a positive impact. The
particles attach themselves to the raindrops and snowflakes and land on the ground where they are less likely to enter
our airways. The environmental temperature also has an effect, as fine dust particles have a different shape in the
summer than they have in the winter, as the temperature is a catalyst for chemical reactions with air pollutants such as
ozone which changes the shape and properties of the particle. (J.P. Close, personal communication, March 7, 2023)

### Conclusion - Which city problem will we be focussing on?

Based on this research and the available data I made a SWOT analysis for each domain to choose from.
![Fine Dust SWOT](/assets/fine-dust-swot.png)
![Biodiversity SWOT](/assets/biodiversity-swot.png)  
![Air Temperature / Water SWOT](/assets/Airtemp-water-swot.png)  
![Air Pollutant Emissions SWOT](/assets/air-pollutant-emissions-swot.png)

Though Simona had done her own research into what was possible, she agreed with picking Fine Dust as this was the one
with the most potential for success and would incorporate AR and AI the best.

The second choice was Biodiversity, using a model to predict which plants would be at a certain location based on
temperature, wind, precipitation etc. However, this would mean a lot of AR models of plants, and seeing as there wasn't
a concrete dataset yet that we had found that we could use we decided that we better go for fine dust as we could expand
to air pollutant emissions at a later date based on the basic zoom lens application that would be created for fine dust.

Air pollutant emissions didn't give enough room for prediction as the data was essentially yearly. Air temperature and
water had too much unreliable data and didn't show the problem direct enough.

With fine dust, we can make time-based models, but also models that use wind, temperature and humidity and similar
features so people can adjust these values themselves and create their own environment or 'future' scenario.

## 2. Visualization

As per the requirement of the City Digital Twin project, we need to incorporate Augmented Reality into the project.
Through this, we can make data more comprehensible for regular people in the city of Eindhoven and can display things
that are normally not visible such as Fine dust particles.

Augmented Reality is defined as a real-time direct or indirect view of a physical real-world environment that has been
enhanced/augmented by adding virtual computer-generated information to it.

Augmented Reality brings virtual information to the user's immediate surroundings and any indirect view of the
real-world environment. It enhances the user's perception of and interaction with the real world. Instead of completely
immersing the user in a synthetic world, AR technology augments the sense of reality by superimposing virtual objects
and cues upon the real world in real-time. AR could potentially apply to all senses and can be used to augment or
substitute the user's missing senses by sensory substitution. AR applications can also remove real objects from the
environment. This is usually called mediated or diminished reality. In this case, the object is covered with virtual
information so the user has the impression that the object is not there (Carmigniani & Furht,

2011) [<sup>12</sup>](#^source12-resourcedoc).

### 2.1 How can this problem be visualized?

#### Concept Designs

To visualize this problem, based on the requests and feedback from the stakeholder and expert we started by creating
sketches.
{Scan and upload sketches}

After creating these sketches, we talked them through with the stakeholder. She said that the zoom-in designs both look
good, and the zoomed-in version looks the best. A good idea is to add a 'confidence level' to the designs to indicate
how
accurate the visualisation is. Based on this feedback, we adjusted the sketches into Figma clickable wireframes.
![Initial Designs 1](/assets/initial-designs-app1.png)
![Initial Designs 2](/assets/initial-designs-app2.png)

These were then shown to Media Semester 6 students and based on their feedback, as referenced in
the [Feedback Report](https://s-vermeer.github.io/documents/feedback-report#media-design-students) the designs were
adjusted again. Their main feedback was to make the text larger, add icons to labels, replace text with icons and add
labels to existing icons. They also mentioned that the sliders should be moved to the bottom to make it more
ergonomic. That is why in the updated designs the info screen has a scroll functionality to account for the bigger
text. There is a combination of icons and images, with a settings screen to allow the user the autonomy to remove one
or the other.

![adjusted designs app](/assets/adjusted-designs-app.png)

I checked these designs with my stakeholder, and they agreed that this was the design we were going to use.

### 2.2 - What is the best Augmented Reality technique for this application?

#### Types of Augmented Reality

These types are based on the research of Aytaç Sarkmaz (_Frontend_,
n.d.) [<sup>13</sup>](^source13-resourcedoc), to have a limited pool on what to focus on.
There are two categories of AR, marker-based AR and markerless AR. The difference lies in the information source for the
position of the digital content within the user's view.

##### Marker-based

Marker-based is based on the principle that the system needs to know where the user is and the position that the
user wants to focus. This could be done by adding a predefined marker in the environment, which can be in the form of a
2D picture, QR code, or 3D object. Using this marker, computer vision techniques can be used to detect it, a process
called 'tracking'. This marker makes it easier to identify the position of the virtual objects to be displayed (
Boonbrahm et al., 2020) [<sup>14</sup>](#^source14-resourcedoc).

**Face-AR**  
In the case of Face-AR, your face is the marker. Using face detection, a mask-like image is placed over your face,
altering your facial features in a realistic or unrealistic way. Face detection uses local features such as your eyes
and nose to identify the dimensions on your face and adjust the overlay accordingly as well as track your face when it
is moving (Ahmed, M. N., 2016) [<sup>15</sup>](#^source15-resourcedoc).

##### Markerless AR

With markerless AR, a combination fo GPS and the compass sensor on the smartphone is used to place the virtual object at
any position within the range of the camera view. An example of this is Pokémon Go. There are also platforms that use
data from a device's motion sensor and camera to identify spatial movement across six axes. This makes it so virtual
objects remain accurately positioned in the real world. It also adds a plane detection function, so the virtual items
can be placed onto surfaces in a realistic way (Boonbrahm et al., 2020) [<sup>14</sup>](#^source14-resourcedoc)

**Location-based**  
Location-based Augmented Reality revolves around localizing the user and placing a 3D object somewhere relative to the
user's location. The localization can be done through various methods based on the requirements.

Localization systems are constantly evolving. Early versions used radio signals and a remote server to determine the
user's location. Radio Frequencies (RF) tend to be able to reach where regular GPS fails, such as indoors and in urban
environments due to their higher power levels, wider coverage and their ability to penetrate buildings due to their
lower frequencies. However, they usually do not carry timing, which is critical in range calculation and they are prone
to degrading due to other signals. They were also not made for localization (Moghtadaiee et
al.,2011) [<sup>16</sup>](#^source16-resourcedoc)

Simultaneous Localization and Mapping (SLAM) approaches are mostly developed for robots, and use a variety of sensors to
position a robot in an environment, using things such as sound, infrared and landmarks to map the environment in real
time (Chong et al., 2015) [<sup>17</sup>](#^source17-resourcedoc)

The 6 Degrees of Freedom (6DOF) pose estimation uses the 6 axes that a rigid body can move in a 3D space to estimate the
position relative to for example images from a dataset.

However, the most widespread is standard GPS localization, combined with the positioning of the phone through its native
sensors and surface detection (Paucher & Turk, 2010) [<sup>18</sup>](#^source18-resourcedoc)

**Projection Based / Spatial AR**  
Spatial AR uses projections to augment the physical environment of the user regardless of their field of view. There are
several versions of this, such as with and without head tracking. Without head tracking, the images do not constantly
change while the user moves. With head tracking, the images are updated dynamically (Behringer et al.,

1999) [<sup>19</sup>](#^source19-resourcedoc)

#### Frameworks / Platforms

There are a variety of frameworks and platforms to develop AR with. That is why we narrowed it down to the frameworks
researched by Aytaç Sarkmaz (_Frontend_, n.d.) [<sup>13</sup>](#^source13-resourcedoc). The
information was verified in the documentation and adjusted as some of it was outdated.

##### 8th Wall

8th Wall is a complete AR solution. It has a variety of advanced features including SLAM, 6-DoF and responsive scaling.
You can also enable occlusion and physics so you can add interaction between your virtual environment and the physical
world. It is compatible with a large number of 3D frameworks and browsers and is made to integrate with computer vision
technologies like machine learning models and external libraries such as location services. However, it is quite
expensive at $9 - $3000 / month.

##### ARCore

ARCore is Google's AR platform. However, it only works on Android and iOS. It has an understanding of its environment
such
as the size location and debt and allows for motion tracking and anchors. It is free to use.

##### AR.js

AR.js is a lightweight library for AR on the web. It will run on any phone with WebGL and WebRTC. Something good to know
is that Firefox does not give accurate device orientation so location-based AR will not work correctly. Chrome might
open the wrong camera when there are multiple camera's on a phone. It also always has to be deployed under HTTPS. It is
free, however, the support is paid. It supports Image-Based, Marker-based and Location-based AR.

##### Banuba

Banuba specializes in Face AR. It has a variety of features related to the face such as tracking, audio, masking, face
morphing and many more. It is supported on multiple platforms, such as Android, iOS, Web, Windows, MacOS and Unity. It
has a free trial but after that it is paid and the final cost is unclear as you only pay for the features you need.

##### Unity

Unity supports combining ARCore, ARkit, and WebAR. You can develop an application for multiple platforms
simultaneously. It supports the key features from multiple platforms, as well as the unity features such as
photorealistic rendering, physics and device optimizations. You can also use Unity as a library to embed it in your
existing mobile app. The pricing ranges from free for individual use to just under 3000 a year per seat for enterprises.

##### WebXR

WebXR is a group of standards which are used together to support VR and AR. However, it is not yet implemented in a lot
of browsers so users would need to download things or edit settings to be able to run it. It is however compatible with
a lot of things such as immersive headsets and AR-Core compatible devices. It also has multiple aspects to make the XR
experience interactive. It appears to be free.

##### WebARkit

WebAR Kit wants to take the best from open-source WebAR and take it to the next level as the ARtoolkit project is dead
and its successor is not web-friendly. However there doesn't appear to be anything concrete yet, and the last update was
April 7, 2021, so it seems that this project is not continuing and not useful yet.

##### Zapworks

Zapworks is a complete toolkit just like 8th Wall. However, it lacks some features but compensates in pricing. It has
trouble anchoring due to not supporting world tracking on the web. It allows the creation of AR experiences without
coding or with low code. It supports a variety of the above-mentioned features and has a 14-day free trial. After that,
it is 58-435
euros per month, with custom pricing for enterprises. It is compatible with Safari and Chrome, and a large amount of
3D JS frameworks. It has image and face tracking.

### 2.2.1 Which Augmented Reality techniques can we work with?

#### Proof of Concepts

Based on our theoretical knowledge of the types of AR and the frameworks we would need to develop them, and our
knowledge of the project, we can say that we will either have to use Location-based AR or have the phone as the marker
and place things relative to the phone. This is because we are working off of coordinate data, but want the
visualization
to be somewhat relative to the user so it is immersive. This could be done by getting the location of the phone,
referencing it to the data we want to visualize, and displaying the 3D models around the user.

If we want to use location-based, our options for development are 8th Wall or AR.js, and seeing as AR.js is free this is
definitely an option. However, it is also known to not be extremely user-friendly when developing, so if that doesn't
work out, we can also work with Unity in a non-location-based application way. Unity implements ARCore and a variety of
features and would also be free to develop with, along with a friendlier user interface.

However, before we can decide what we will be using, we need to make a few proof of concepts to see whether they will
work. Something to note is that the development of these concepts happened mostly simultaneously, so some aspects do
overlap.

##### Unity

To start experimenting with Unity, I followed a tutorial on Augmented Reality with Unity. The tutorial that was
provided by Unity itself was throwing a lot of errors, so we decided to approach it differently and use a (_Create an AR
Game Using Unity’s AR Foundation_, n.d.)[<sup>20</sup>](#^source20-resourcedoc). We got the
camera to work and have a model display at the location of the camera. However, initially, it was just stuck in the
position where the camera pointed first.

There is an error in the tutorial, where it is missing some code. By using a Youtube tutorial, we were able to find the
error area and alter the code. (Indra M, 2022)[<sup>21</sup>](#^source21-resourcedoc) We also
figured out that we were using the wrong 'version', as we needed one that used the Universal Render Pipeline so we had
the ForwardRenderer Script. After recreating the project using the correct setup, we were able to correct this, so it
followed the camera and stuck to whatever surface was detected.

This meant that we had a simple form of AR. However, as we are not experienced in Unity, setting up an entire
application around it would be extremely time-consuming, so we decided that if Unity was going to be continued to
develop, we would need a way to integrate it into our React Application.

##### React

As we had a base setup for a web-based react application, the first goal was to see whether we could get augmented
reality working within this application. However, this application was Javascript based, we couldn't just use the
tutorials for Aframe and AR.js as these were HTML/CSS based.

After getting the camera to work somewhat, as it didn't work on mobile yet, we started experimenting with a method to
see whether we could get something to display. Things such as an Aframe package (_Aframe-React_,
2018)[<sup>22</sup>](#^source22-resourcedoc) were not working, as nothing was showing up.
Trying to get HTML code to work was also not having the desired effect.

Seeing as Unity was working, and during our search for ways to get AR in React we found a few packages that used Unity
to display AR in React, we decided to focus on trying to integrate Unity into React.

For integrating Unity into React there are a variety of packages such as react-native-unity-view-reinvented (
_React-Native-Unity-View-Reinvented_, 2019)[<sup>23</sup>](#^source23-resourcedoc),
react-native-unity-view (Madsen, 2019/2023)[<sup>24</sup>](#^source24-resourcedoc),
react-native-unity (_@azesmway/React-Native-Unity_,
2022)[<sup>25</sup>](#^source25-resourcedoc), react-native-unity-play (
_React-Native-Unity-Play_, 2021)[<sup>26</sup>](#^source26-resourcedoc) and
react-unity-webgl (_React-Unity-Webgl_, 2023)[<sup>27</sup>](#^source27-resourcedoc). However,
as soon as we started working on this, version conflicts started occurring. It appears that either the version of React
we are working in was too new, or in general the ability to get Unity working in React was deprecated.

We got react-native-unity to somewhat work, in the sense that there were no version conflicts that broke the starting of
the application, however in this case the importing of the Unity project was the problem. The Gradle kept not being able
to reference the Unity project, despite us following multiple tutorials and troubleshooting and even creating a new
project to keep the differences to the bare minimum. Nothing was working.

##### HTML/CSS and AR.JS

For AR.js we started by trying to do the location-based tutorials for AR.js (_Image Tracking and Location-Based AR with
A-Frame and AR.Js 3_, n.d.).[<sup>28</sup>](#^source28-resourcedoc). However, this didn't
appear to work as no matter what we tried, nothing was showing up on the screen, aside from the camera.

Based on this, we went 'back to basics' and worked on marker-based AR. As we could use it for the Poster Market and
to see whether the most well-documented forms of AR did work. For this, we used the tutorial for MindAR (_Build a
Marker-Based AR App in 6 minutes | Source Code included | kandi tutorial_,
n.d.).[<sup>29</sup>](#^source29-resourcedoc) This tutorial came with models and working code,
which could be altered to fit our needs. Based on this application, after changing it to use our own marker and model,
we were able to make a POC of marker-based AR.

However, though it meant that we could make AR work, it wasn't useable for our application as that needed to be
markerless. Based on that we started working on a location-based prototype. Around that same time, we got into contact
with a teacher that is also working on AR, Bernd-Jan. He has been working on a location-based prototype. After finding
that our testing phone was not ideal for AR, we decided to get a phone from the ISSD so we could focus on the software
instead of the technology. We also got access to their location-based prototype, so we could experiment with it. Based on
that we were able to make a location-based prototype and a relative to the user prototype.

### Conclusion

Though there are a variety of options, based on our current needs and options that work, the best AR technique for right
now is AR.js. It is the only one out of our options that works, despite our research saying it is not user-friendly to
work with. We experienced that during the start-up phase, however, once we were able to get something to show, it was
relatively easy to work with. It is free, online so it is widely available as long as your phone allows for AR and
allows for easy integration into a web application.

Our version is best described as Markerless AR and could be specified as Location-based although it doesn't fit the
official description. If React or Unity work, it might be possible to create a downloadable application through them,
however for now the AR.js application functions on both laptops and phones which is how we can make it the most
accessible currently.

## 3. Artificial Intelligence

### 3.1 What data is available regarding this problem?

Eindhoven location-based data visualization is a requirement from the stakeholder, however to know what we are
visualizing we need to do some *Data Analytics*.

### 3.2 How can AI be used to make predictions using this data?

As this project is done in the context of an AI internship, artificial intelligence is also an important aspect. To see
which AI will be created, some *Data Analytics* is needed and based on that a Prototype will be created. After testing
that prototype we can see whether the predictions have the desired effect.
<div style="page-break-after: always;"></div>  

## 4. User Experience

### 4.1 What devices will the AR application be available on?

In our requirements we have that it should be available on a phone. This will detail which devices will be supported and
why. It will also detail how these decisions were made.

### 4.2 How can it be verified whether users understand the severity of the city problem?

To verify whether the intended users can understand our product, we will be using a mix of *Observation*, to see what
our users behave like, and various user testing methods to see how the user responds to the application.
<div style="page-break-after: always;"></div>  

## Conclusion

Answer: How can the severity of city problems in Eindhoven be visualized using AR in a way that is accessible and
understandable to the users?
<div style="page-break-after: always;"></div>  

## References

[<sup>1</sup>]:  _Explore—Eindhoven Open Data_. (n.d.). Retrieved 10 February 2023,
   from [https://data.eindhoven.nl/explore/?refine.theme=Nature+and+environment&sort=modified](https://data.eindhoven.nl/explore/?refine.theme=Nature+and+environment&sort=modified)
   ^source1-resourcedoc
2. _Impacts of nature-based solutions on the urban atmospheric environment: A case study for Eindhoven, The Netherlands
   Elsevier Enhanced Reader_. (
   n.d.). [https://doi.org/10.1016/j.ufug.2020.126870](https://doi.org/10.1016/j.ufug.2020.126870)  ^source2-resourcedoc
3. McDonald, R. I., Mansur, A. V., Ascensão, F., Colbert, M., Crossman, K., Elmqvist, T., Gonzalez, A., Güneralp, B.,
   Haase, D., Hamann, M., Hillel, O., Huang, K., Kahnt, B., Maddox, D., Pacheco, A., Pereira, H. M., Seto, K. C.,
   Simkin, R., Walsh, B., … Ziter, C. (2020). Research gaps in knowledge of the impact of urban growth on biodiversity.
   _Nature Sustainability_, _3_(1), Article
    1. [https://doi.org/10.1038/s41893-019-0436-6](https://doi.org/10.1038/s41893-019-0436-6) ^source3-resourcedoc
4. Pope, C. A., Ezzati, M., & Dockery, D. W. (2009). Fine Particulate Air Pollution and US County Life Expectancies.
   _The New England Journal of Medicine_, _360_(4),
   376–386. [https://doi.org/10.1056/NEJMsa0805646](https://doi.org/10.1056/NEJMsa0805646) ^source4-resourcedoc
5. Wang, W., Cai, C., Lam, S. K., Liu, G., & Zhu, J. (2018). Elevated CO2 cannot compensate for japonica grain yield
   losses under increasing air temperature because of the decrease in spikelet density. _European Journal of Agronomy_,
   _99_, 21–29. [https://doi.org/10.1016/j.eja.2018.06.005](https://doi.org/10.1016/j.eja.2018.06.005)
   ^source5-resourcedoc
6. _Chapter 1—Global Warming of 1.5 oC_. (n.d.). Retrieved 13 February 2023,
   from [https://www.ipcc.ch/sr15/chapter/chapter-1/](https://www.ipcc.ch/sr15/chapter/chapter-1/) ^source6-resourcedoc
7. _Climate Change: Global Sea Level NOAA Climate.gov_. (n.d.). Retrieved 13 February 2023,
   from [http://www.climate.gov/news-features/understanding-climate/climate-change-global-sea-level](http://www.climate.gov/news-features/understanding-climate/climate-change-global-sea-level)
   ^source7-resourcedoc
8. Hooper, D. U., Adair, E. C., Cardinale, B. J., Byrnes, J. E. K., Hungate, B. A., Matulich, K. L., Gonzalez, A.,
   Duffy, J. E., Gamfeldt, L., & O’Connor, M. I. (2012). A global synthesis reveals biodiversity loss as a major driver
   of ecosystem change. _Nature_, _486_(7401),
   105–108. [https://doi.org/10.1038/nature11118](https://doi.org/10.1038/nature11118)^source8-resourcedoc
9. Cardinale, B. J., Duffy, J. E., Gonzalez, A., Hooper, D. U., Perrings, C., Venail, P., Narwani, A., Mace, G. M.,
   Tilman, D., Wardle, D. A., Kinzig, A. P., Daily, G. C., Loreau, M., Grace, J. B., Larigauderie, A., Srivastava, D.
   S., & Naeem, S. (2012). Biodiversity loss and its impact on humanity. _Nature_, _486_(7401),
   59–67. [https://doi.org/10.1038/nature11148](https://doi.org/10.1038/nature11148)^source9-resourcedoc
10. Otjes, R., & Close, J. P. (Directors). (2023, February 8). STAD VAN MORGEN - Special Guest: René Otjes—08.02.2023
    HQ. In _Stad van Morgen_.
    RARARADIO. [https://www.mixcloud.com/RARARADIO_EHV/stad-van-morgen-special-guest-rené-otjes-08022023/?play=fb](https://www.mixcloud.com/RARARADIO_EHV/stad-van-morgen-special-guest-rené-otjes-08022023/?play=fb)
    ^source10-resourcedoc
11. Ritz, B., Hoffmann, B., & Peters, A. (2019). The Effects of Fine Dust, Ozone, and Nitrogen Dioxide on Health.
    _Deutsches Ärzteblatt International_, _116_(51–52),
    881–886. [https://doi.org/10.3238/arztebl.2019.0881](https://doi.org/10.3238/arztebl.2019.0881)
    ^source11-resourcedoc
12. Carmigniani, J., & Furht, B. (2011). Augmented Reality: An Overview. In B. Furht (Ed.), _Handbook of Augmented
    Reality_ (pp. 3–46). Springer New
    York. [https://doi.org/10.1007/978-1-4614-0064-6_1](https://doi.org/10.1007/978-1-4614-0064-6_1)
    ^source12-resourcedoc
13. _Frontend_. (n.d.). Retrieved 6 March 2023, from [https://itouchi.github.io/](https://itouchi.github.io/)
    ^source14-resourcedoc
14. Boonbrahm, S., Boonbrahm, P., & Kaewrat, C. (2020). The Use of Marker-Based Augmented Reality in Space Measurement.
    _Procedia Manufacturing_, _42_,
    337–343. [https://doi.org/10.1016/j.promfg.2020.02.081](https://doi.org/10.1016/j.promfg.2020.02.081)
    ^source14-resourcedoc
15. Ahmed, M. N. (2016). Face Detection based Attendance marking
    System.. https://www.researchgate.net/profile/Varsha-H-S/publication/337672627_Face_Detection_based_Attendance_marking_System/links/5de4ab4492851c83645a14d5/Face-Detection-based-Attendance-marking-System.pdf
    ^source15-resourcedoc
16. Moghtadaiee, V., Dempster, A. G., & Lim, S. (2011). Indoor localization using FM radio signals: A fingerprinting
    approach. _2011 International Conference on Indoor Positioning and Indoor Navigation_,
    1–7. [https://doi.org/10.1109/IPIN.2011.6071932](https://doi.org/10.1109/IPIN.2011.6071932)^source16-resourcedoc
17. Chong, T. J., Tang, X. J., Leng, C. H., Yogeswaran, M., Ng, O. E., & Chong, Y. Z. (2015). Sensor Technologies and
    Simultaneous Localization and Mapping (SLAM). _Procedia Computer Science_, _76_,
    174–179. [https://doi.org/10.1016/j.procs.2015.12.336](https://doi.org/10.1016/j.procs.2015.12.336)
    ^source17-resourcedoc
18. Paucher, R., & Turk, M. (2010). Location-based augmented reality on mobile phones. _2010 IEEE Computer Society
    Conference on Computer Vision and Pattern Recognition - Workshops_,
    9–16. [https://doi.org/10.1109/CVPRW.2010.5543249](https://doi.org/10.1109/CVPRW.2010.5543249)^source18-resourcedoc
19. Behringer, R., Klinker, G., & Mizell, D. (1999). _Augmented Reality: Placing Artificial Objects in Real Scenes_. CRC
    Press. ^source19-resourcedoc
20. _Create an AR game using Unity’s AR Foundation_. (n.d.). Google Codelabs. Retrieved 7 March 2023,
    from [https://codelabs.developers.google.com/arcore-unity-ar-foundation](https://codelabs.developers.google.com/arcore-unity-ar-foundation).
    ^source20-resourcedoc
21. Indra M (Director). (2022, June 8). _Create an AR game using Unity’s AR Foundation (Car Driving) | Unity 2020 (
    Codelab Tutorial)_. [https://www.youtube.com/watch?v=-wWOktZCSX0](https://www.youtube.com/watch?v=-wWOktZCSX0)
    ^source21-resourcedoc
22. _Aframe-react_. (2018, February 12).
    Npm. [https://www.npmjs.com/package/aframe-react](https://www.npmjs.com/package/aframe-react) ^source22-resourcedoc
23. _React-native-unity-view-reinvented_. (2019, December 2).
    Npm. [https://www.npmjs.com/package/react-native-unity-view-reinvented](https://www.npmjs.com/package/react-native-unity-view-reinvented)
    ^source23-resourcedoc
24. Madsen, A. S. (2023).
    _React-native-unity-view_ [C#]. [https://github.com/asmadsen/react-native-unity-view](https://github.com/asmadsen/react-native-unity-view) (
    Original work published 2019) ^source24-resourcedoc
25. _@azesmway/react-native-unity_. (2022, November 25).
    Npm. [https://www.npmjs.com/package/@azesmway/react-native-unity](https://www.npmjs.com/package/@azesmway/react-native-unity)
    ^source25-resourcedoc
26. _React-native-unity-play_. (2021, June 7).
    Npm. [https://www.npmjs.com/package/react-native-unity-play](https://www.npmjs.com/package/react-native-unity-play)
    ^source26-resourcedoc
27. _React-unity-webgl_. (2023, January 6).
    Npm. [https://www.npmjs.com/package/react-unity-webgl](https://www.npmjs.com/package/react-unity-webgl)
    ^source27-resourcedoc
28. _Image Tracking and Location-Based AR with A-Frame and AR.js 3_. (n.d.). A-Frame. Retrieved 20 April 2023,
    from [https://aframe.io](https://aframe.io) ^source28-resourcedoc
29. _Build a Marker-Based AR App in 6 minutes | Source Code included | kandi tutorial_. (n.d.). Retrieved 20 April 2023,
    from [https://www.youtube.com/watch?v=iSIj2KFwVT4](https://www.youtube.com/watch?v=iSIj2KFwVT4)
    ^source29-resourcedoc