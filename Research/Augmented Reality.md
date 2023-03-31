
*Definition*:
A real-time direct or indirect view of a physical real-world environment that has been enhanced/augmented by adding virtual computer-generated information to it [1] {Augmented Reality: An Overview Julie Carmigniani and Borko Furh}. 

##### Milgram's Reality-Virtuality Continuum
![Image of Reality-Virtuality Continuum](RVContinuum.png)
_Fig 1.1 - Visual representation of Milgram's Reality-Virtuality Continuum_

The relationship that Mixed Reality has with reality and a virtual environment, is defined using Milgram's Reality-Virtuality Continuum as simplified pictured in _Fig 1.1_.  Real Environments, at one end of the continuum, consist solely of real objects. This is for example when you have a direct or indirect observation of a real-world scene. At the other extreme are Virtual Environments, which are solely virtual objects such as what you can see in a graphic simulation. To best view a Mixed Reality environment you need to present both the real environment and the virtual one in a single display. [2] {A TAXONOMY OF MIXED REALITY VISUAL DISPLAYS Paul Milgram, Fumio Kishino}

Augmented Virtuality (AV) is closer to a pure virtual environment while Augmented Reality (AR) is closer to the real world. It brings virtual information to the user's immediate surroundings and any indirect view of the real-world environment. It enhances the user's perception of and interaction with the real world. Instead of completly immersing the user in a synthetic world, AR technology augments the sense of reality by superimposing virtual objects and cues upon the real world in real time. AR could potentially apply to all senses and can be used to augment or substitute user's missing senses by sensory substitution. AR applications can also remove real objects from the environment. This is usually called mediated or diminished reality. In this case the object is covered with virtual information so the user has the impression that the object is not there.[1] {Augmented Reality: An Overview Julie Carmigniani and Borko Furh}

### Types of Augmented Reality
These types are based on the research of Aytaç Sarkmaz {RESEARCH}, to have a limited pool on what to focus on. There are two categories of AR, marker-based AR and markerless AR. The difference lies in the information source for the position of the digital content within the user's view.

#### Marker-based
Marker-based is based around the principle that the system needs to know where the user is and the position that the user wants to focus. This could be done by adding a predefined marker in the environment, which can be in the form of a 2D picture, QR code, or 3D object. Using this marker, computer vision techniques can be used to detect it, a process called 'tracking'. This marker makes it easier to identify the position of the virtual objects to be displayed. 

{The Use of Marker-Based Augmented Reality in Space Measurement Salin Boonbrahma , Poonpong Boonbrahma , and Charlee Kaewrata}

##### Face-AR
In the case of Face-AR, your face is the marker. Using face detection, a mask-like image is placed over your face, altering your facial features in a realistic or unrealistic way. Face detection uses local features such as your eyes and nose to identify the dimensions on your face and adjust the overlay accordingly as well as track your face when it is moving. { Face Detection based Attendance marking System Varsha H.S1 ,Vidya G.M1 ,Pranav Shankar1 , Mohammed Nimer Ahmed}

#### Markerless AR
With markerless AR, a combination fo GPS and the compass sensor on the smartphone is used to place the virtual object at any position within the range of the camera view. An example of this is Pokémon Go. There are also platforms that use data from a device's motion sensor and camera to identify spatial movement across six axes. This makes it so virtual objects remain accurately positioned in the real world. It also adds a plane detection function, so the virtual items can be placed onto surfaces in a realistic way. {The Use of Marker-Based Augmented Reality in Space Measurement Salin Boonbrahma , Poonpong Boonbrahma , and Charlee Kaewrata}

##### Location-based
Location-based Augmented Reality revolves around localizing the user and placing a 3D object somewhere relative to the users location. The localization can be done through various methods based on the requirements. 

Localization systems are constantly evolving. Early versions used radio signals and a remote server to determine the user location. Radio Frequencies (RF) tend to be able to reach where regular GPS fails, such as indoors and in urban environments due to their higher power levels, wider coverage and their ability to penetrate buildings due to their lower frequencies. However, they usually do not cary timing, which is critical in range calculation and they are prone to degrading due to other signals. They were also not made for localization.  {Indoor localization using FM radio signals: A fingerprinting approach}. 

Simultaneous Localization and Mapping (SLAM) approaches are mostly developed for robots, and use a variety of sensors to position a robot in an environment, using things such as sound, infrared and landmarks to map the environment in real time {Sensor Technologies and Simultaneous Localization and Mapping (SLAM) T.J. Chong, X.J. Tang, C.H. Leng, M. Yogeswaran∗ , O.E. Ng, Y.Z. Chong}. 

The 6 Degrees of Freedom (6DOF) pose estimation uses the 6 axes that a rigid body can move in a 3D space to estimate the position relative to for example images from a dataset. 

However, the most widespread is standard GPS localization, combined with the positioning of the phone through its native sensors and surface detection. {Location-based augmented reality on mobile phones Remi Paucher and Matthew Turk}.

##### Projection Based / Spatial AR
Spatial AR uses projections to augment the physical environment of the user regardless of their field of view. There are several versions of this, such as with and without head tracking. Without head tracking, the images do not constantly change while the user moves. With head tracking, the images are updated dynamically. {Spatially Augmented Reality Ramesh Raskar, Greg Welch, Henry Fuchs}

#### Frameworks / Platforms
These frameworks are based on the research of Aytaç Sarkmaz {RESEARCH}. The information was verified in the documentation and adjusted as some of it was outdated.

##### 8th Wall
8th Wall is a complete AR solution. It has a variety of advanced features including SLAM, 6-DoF and responsive scaling. You can also enable occlusion and physics so you can add interaction between your virtual environment and the physical world. It is compatible with a large number of 3D frameworks and browsers and is made to integrate with computer vision technologies like machine learning models and external libraries such as location services.. However, it is quite expensive at $9 - $3000 / month.

##### ARCore
ARCore is Google's AR platform. However, it only works on Android and iOS. It has understanding of its environment such as the size location and debt and allows for motion tracking and anchors. It is free to use. 

##### AR.js
AR.js is a lightweight library for AR on the web. It will run on any phone with WebGL and WebRTC. Something good to know is that Firefox does not give accurate device orientation so location based AR will not work correctly. Chrome might open the wrong camera when there are multiple camera's on a phone. It also always has to be deployed under https. It is free, however the support is paid. It supports Image-Based, Marker-based and Location-based AR.

##### Banuba
Banuba specializes in Face AR. It has a variety of features related to the face such as tracking, audio, masking, face morphing and many more. It is supported on multiple platforms, such as Android, iOS, Web, Windows, MacOS and Unity. It has a free trial but after that it is paid and the final cost is unclear as you only pay for the features you need.

##### Unity
Unity supports combining ARCore, and ARkit, and WebAR. You can develop an application for multiple platforms simultaneously. It supports the key features from the multiple platforms, as well as the unity features such as  photorealistic rendering, physics and device optimizations. You can also use Unity as a library to embed it  in your existing mobile app. The pricing ranges from free for individual use to just under 3000 a year per seat for enterprises. 

##### WebXR
WebXR is a group of standards which are used together to support VR and AR. However it is not yet implemented in a lot of browsers so users would need to download things or edit settings to be able to run it. It is however compatible with a lot of things such as immersive headsets and AR-Core compatible devices. It also has multiple aspects to make the XR experience interactive. It appears to be free.

##### WebARkit
WebAR Kit wants to take the best from open source WebAR and take it to the next level as the ARtoolkit project is dead and its successor is not web friendly. However there doesn't appear to be anything concrete yet, and the last update was April 7, 2021 so it seems that this project is not continuing and not useful yet.

##### Zapworks
Zapworks is a complete toolkit just like 8th Wall. However it lacks some features but compensates in pricing. It has trouble anchoring due to not supporting world tracking on web. It allows to create AR experiences without coding or with low code. It supports a variety of the above mentioned features, and has a 14 day free trial. After that it is 58-435 euros per month, with a custom pricing for enterprises. It is compatible with safari and chrome, and a large amount of 3D JS frameworks. It has image and face tracking.

### Conclusion
With the information we have on frameworks and tools, knowing that we will need location-based support, we can choose between 8th Wall with SLAM and 6-DoF, or AR.js. Seeing as AR.js is free and allows for the same compatibililty as the paid 8th Wall, we will use AR.js for now. If it turns out that we need features that are not supported by AR.js, we can reevaluate whether we need to change to 8th Wall, currently it doesn't seem necessary. 

-----
### 2.1 How can this problem be visualized?
Once the city problem is determined, how it can be visualized is the next most important thing, which is also regarding the *Problem Analysis* as defining the exact problem here is crucial. In addition to this some type of research through design will be done, however the method for this is undetermined so far.
<div style="page-break-after: always;"></div>

### 2.2 What is the best Augmented Reality technique for this application?
Augmented Reality is a requirement from the stakeholder, however for further development and to be able to design and develop accordingly, a *Literature Study* needs to be done as to the different techniques of AR and how they fit in this context. This can be substantiated with some form of user testing such as *Usability Testing*.
<div style="page-break-after: always;"></div>
