---
layout: page
title: Personal Reflection
permalink: /personal-reflection/
nav_order: 4
---

## Personal Reflection

#### Professional Duties

You carry out the professional duties on a junior bachelor level resulting in professional products in line with the
IT-area you are working in.

In the section below I will detail which products I delivered and how they relate to the HBO-i contexts Analysis,
Design, Realize, Advise and Manage&Control

{Show professional products + ex. IT festival}

##### Analysis

A lot of this project was analysis. There was a lot of literature research and data analysis. Essentially, the project
was analysing the problem and advising on the solution based on a prototype.

For this section I will be focusing on the Research Document and the EDA. These are the two documents that display the
Analysis component the most. This doesn't mean that there were no other analyses, but these were the most important
ones.

The first aspect I analysed were the problems in Eindhoven. This analysis was based around the problems that were on
the [Eindhoven Open Data](https://data.eindhoven.nl/pages/home/) portal, and connecting these to their origins:
Population Growth and Urban Growth. Using Literature Analysis I managed to make a comprehensive analysis of the problems
and using SWOT I was able to choose a problem to scope the project with. My company coach thought that I made a clear,
useful and educational overview of the city problems and agreed that I made it clear why I chose Fine
Dust. ![Feedback Simona Research Document 27 Feb](/assets/feedback-simona-27-2-rd.png). Of course, I also processed the
feedback about the structure for 1.2. I believe that I did an extremely good job at this analysis. Of course, not
everyone will be able to comprehend the research. However, anyone who is working on the project will be up to speed on
the different problems, their origins, why it is a problem and why we chose fine dust. It uses multiple techniques,
literature research, expert interviews and SWOT analysis, and is still properly scoped.

For the visualization aspect, aside from the designs I also researched Augmented Reality, analysed the different methods
and frameworks to advice on the best one. For this, I scoped the techniques and frameworks using
the [research of Aytaç Sarkmaz](https://itouchi.github.io/). After I did this analysis, I moved towards testing my
preferred methods. The practical research revealed that I needed to re-analyse as the advice from the initial analysis
wasn't working. I believe I could have been more concise with my research, as it didn't need as much time spent on it.
However, with the time that was spent on it, I believe I gave a clear overview of our choices and what we based our
decisions on. Essentially, it was a bigger analysis than needed but the content is up to standards.

{analysis user predictions}
{EDA in Research Doc}

The Exploratory Data Analysis was a feat in itself. Though it isn't up to my usual standards of an EDA, a detailed,
transparent, reproducible analysis on several levels of the data, this had a valid reason. Due to the inconsistencies in
the data from TNO, we needed to shift our focus to the report that TNO made on this same data. It is always good to
analyse the data yourself, but after our findings weren't consistent with our theoretical knowledge we decided to
compare it to the report of TNO. This revealed even more inconsistencies which shifted the focus of our project towards
trying to figure out what inconsistencies we could find and discuss them with TNO. Initially we analysed the raw data of
a single location. This revealed some long periods of time when there was no data recorded and a few spikes. Between
PM2.5 and PM1 there was limited difference visible in comparison to PM10. After a basic analysis of a single location,
we decided to combine the locations to see if we can observe patterns from there. Here we also find some irregularities
so we decided to pivot towards comparing it to the TNO report. This analysis of the TNO report was a valuable addition
to both the City Digital Twin project, our expert Jean-Paul and TNO itself. For the CDT project, it was useful as it
showed irregularities in otherwise trusted data. Jean-Paul is part of the maintenance of the network and they were
unaware of the inconsistencies of the TNO API, whether they were a problem at sensor level or API level. For TNO it was
interesting as they too were unaware that there were problems within the API, as well as with the location data that was
shared online. The analysis isn't perfect, but it was mainly meant to be a quick way of communication. Simona also noted
that this EDA was good and that I didn't need to work on reordering, improving and expanding it. If I had more time, I
would have highlighted the differences between the airport, urban and agricultural groups. I would also have reevaluated
the data quality after the insights from TNO and prepared the data in such a way that the data from TNO only is the
correct data. A heatmap of the different locations would also be an interesting visualization. All in all there are
plenty of ways the EDA could have been expanded, but within the time scope of the project, this is the final result. It
is a valued analysis of the data, including a comparison to the TNO report.

Based on this analysis we contacted TNO and after a meeting we focused on modelling. Here there was an analysis as well,
which model was the best fit for the project. For this we analysed ARIMA, ETS, PROPHET, SNAIVE, Seasonal ARIMA and
Polynomial features. I did this based on a previous project from semester 4 which also uses time series. The Polynomial
Features were added on advice of Simona. The selection of different models is extensive, however the descriptions and
annotations of the results are minimal. The modelling analysis could have been expanded with different levels of
grouping, such as every hour, every two hours, short-term predictions, long-term predictions and the likes. The model
that is currently there is good and functions as intended and it is clear why it is chosen. However, it is by no means a
complete analysis.

##### Design

I designed an application, sketching, then showing it to semester 6 Media students and improving it. With my current
knowledge level, experience and scope, these are quite good designs. I have gone through several iterations, tested
them, and verified them with my company coach, users and design experts. I could have focused more on the designs, only
creating a prototype, but seeing as I also needed to incorporate AR and AI, with the current scope this level of design
is optimal. I have already advised future designers based on the feedback that was received so they could focus on it
completely, not having to worry about the integration of the different components or using mock data but simply
improving the front-end designs.

I also designed the software architecture that has a front-end, a back-end that uses an API and that connects to the
external API's such as from TNO. There was no existing system, but the way I built this system it is scalable and can
integrate new components quickly. It isn't the optimal system, as a second storage system is most likely needed, but it
is a flexible, scalable system that is easy to use and continue with.

##### Realize

In my Feedback Report I have shown several demo's of the realized prototype, and the feedback I have obtained on it. It
is not complete, but in my Advice Report I have also detailed aspects that still need to be done and how it can be done.
The front-end can be found at [cdt.skylervermeer.nl](cdt.skylervermeer.nl) and under the
subdomain [cdt-api.skylervermeer.nl](cdt-api.skylervermeer.nl) the API is hosted.

Due to the XR event being a deadline for an integrated AR component, and the IT festival being the deadline for the
integrated AI component the application was fully realized on May 23rd. The scope of the project was completed, though
the exact interpretation of the scope, the planning so to say, was updated throughout the semester. The project was
realized, and documented fully in such a way that students could easily continue the project simply by reading the
documentation.

I am proud that I was able to complete the project in such a way that there is a functional product with full potential
for continuance including the research to back up the choices. This is something that I often struggle with, finding a
proper point to stop. The fact that I managed to do it this early in the semester is admirable.

##### Advise

Though I have advised smaller sections throughout the project, mostly during the conclusions of chapters, my main advise
is visible in the Advice Report. In this report I advise future project members on how to continue the project, dividing
it up into the sections research, design and functionality. I also have identified the student groups that would be a
best fit for each subsection. I have done this advice based on conclusions of research, items that fell outside of the
adjusted scope and acknowledging my own expertise while also identifying areas in the project that need to be improved.

Alongside with that, I have also advised future project members in the Software Architecture Documentation on how they
should proceed during development, initially using the current repository, then when a new server is set up transferring
it to the new repository and server. This allows students to continue with the project with minimal effort.

##### Manage & Control

For Manage & Control, I have detailed my process in the Software Architecture Documentation. In this document, I detail
for the current contexts, the data, the CI/CD and for future development what steps I have taken and what steps other
members might need to take. I have set up the project in such a way that it is easy to manage for me and easy to
continue for future users. I am as transparent as possible and designed the project and application to be scalable. The
biggest factor in this is the CI/CD pipeline. First, the image gets created based on (a directory in) the repository.
Then SonarQube does a scan, and we can add 'requirements' to this scan, such as that it will fail and stop the pipeline
if there are too many code smells. Then, the application gets pushed to the docker repository and to the server so it is
automatically updated. This can be done by anyone who has permission to push to the repository. This helped me, as the
project needs to be hosted on a server due to it needing HTTPS and CORS requiring a pre-determined domain. It allowed me
to not have to worry about pushing and pulling the container, simply focussing on pushing the new commit and seeing
whether the pipeline succeeds.

#### Situation-orientation

There is no doubt whether I delivered relevant results for the project and company. Though I wasn't able to create a
fully functional application, all my research and insights and current progress are highly valuable to the City Digital
Twins project.

The research in itself, on the city problems in Eindhoven is extensive and can be used for future projects that might
use a different domain and have different objectives. The same goes for my research on AR.

As for the data aspect, the data I scraped was used by Simona in a research paper, and my research found inconsistencies
in the API from TNO, which is valuable to our expert, TNO and future and past users of the data.

I used my previous knowledge in AI to pick time series models and tuned them until there was a model that worked. This
is something that had not yet been done within other projects on fine dust.

This all was done within the Fontys Lectorate, where I was almost daily on location, more than was required. Fontys
Lectorate focuses on applied research and the sharing of knowledge. Not only did I use applied research for my
application, I also shared knowledge at several layers of the project. During the knowledge sharing sessions I shared
valuable information, asked insightful questions and provided meaningful feedback to my colleagues. I also shared the
project during the XR event, IT Festival and CDT presentation.

I worked in the environment of the City Digital Twin Projects, and had the company, the expert and the users as
stakeholders. Using my portfolio I created a way for the project to be continued and expand the City Digital Twin
Projects knowledge.

#### Future-Oriented Organisation

This project was made for future development and is aimed at improving the awareness around Fine Dust. It touches upon
several sustainable development goals, and throughout the decision-making process several aspects were considered. In
the sections' organization, the progress rapports and in the ways that the project is prepared for handover it is clear
how I was in control of all aspects of the project and managed them efficiently.

{Insert some decisions, trello etc}

#### Investigative Problem Solving

During this project I had to shift my focus at several points and had to do my best to not get lost trying to solve a
problem and abandoning the goal.

The first problem I encountered was when choosing the focus of the project. This needed to be researched which I did
while making the project plan. To ensure that I didn't spend too much time on this I decided to spend two weeks on it,
with the second week deciding the topic during a meeting with Simona. This allowed me to scope, still spend enough time
on it as it was the foundation of the project but make an educated choice within a reasonable timeframe. It also helped
that we already had the scope of using the Eindhoven Open Data.

Then we had the aspect of Augmented Reality. Based on my literary research the options were React, Unity integrated into
React, and Aframe combined with AR.js. Despite my best efforts I didn't manage to make any prototypes work except for
the Aframe and AR.js combination. I followed tutorials, asked Simona for help and tried out multiple options. Simona
suggested I contact Bernd-Jan, who showed me his prototype based on which I could make my own and helped me get an
appropriate phone from the ISSD so problems I encountered were most likely related to the software instead of the
hardware. So by communicating with my company coach, asking for help and learning when to quit I managed to get a
working Augmented Reality component. Arguably, I could have stopped experimenting with the others earlier, however the
theoretical research indicated that those were a better fit and Aframe and AR.js were more difficult to work with so I
wanted to put a little extra into trying to make the initial best fit work.

The data was a problem in itself. The gathering of the data took quite long. Just under 50 locations needed to be
scraped, the API didn't like the constant requests and crashed multiple times, and in general I could have made better
choices such as only scraping one or a few locations. Alternatively, I could have looked into a desktop computer from
school instead of the laptop that I had to travel with. Luckily the holiday covered most of this period, and I managed
to do some things while the data was scraping, but this was a time-intensive task that could have started smaller.

The analysing of the data was also a problem in and of itself. There wasn't much data aside from the measurements, so
not a lot of new information could be gathered from it, aside from means, the data quality and some correlations between
the locations. However, seeing as I wasn't seeing the trends that the expert had confirmed to me there were, I asked for
help and Simona also took a look. This lead us towards the 2021 TNO report about the sensors. I started comparing their
results with my own, as they should be using the same data. Technically they were, however they took a few additional
steps such as only considering data if it was active during 75% of a timeframe (so 9 months out of a year, 75% of
quartiles etc). Their visualizations were also grouped by the mean per hour. This still didn't account for a lot of the
differences, so we contacted our expert and later TNO for an appointment. Based on this appointment we clarified some of
our own errors or oversights, and we uncovered some errors from TNO that they were unaware of.

Meanwhile I was also struggling with modelling. Most of my time series models were drawing straight lines, and even
those that came somewhat close, only came close for short-term predictions. Based on the meeting with TNO, and our
findings we decided to add weather data. This was only possible within the prophet model. This gave good results,
especially when limiting the predictions to only a few hours into the future. This did require us to continuously
request the most recent data, but overall it gave an acceptable model.

Finally, the deployment. To be able to share it with the outside world, I needed a way to deploy the application. For
this, I used docker to host them. However, I soon encountered CORS errors. After a lot of troubleshooting, trial and
error and research into CORS, I decided to simply move the requests to the back-end, as CORS wouldn't allow the requests
from the front-end. That is when I decided to set up an API, as I most likely would have had to either way to connect
the model to the front-end. After connecting it to my own domain and running it using docker, I was able to use it as a
trusted domain. However, the CORS errors only went away on Google Chrome. Firefox was still giving errors. This was
because I needed to add HTTPS to the application.

Once I did this, using LetsEncrypt, I started focussing on a CI/CD pipeline as from now on any changes would have to be
pushed to the repository to update. For this I needed runners but git.fhict.nl didn't have any. That is why I also set
some up on my own server, where the docker containers were also running. To get this up and running, especially since we
were connecting to an external server, took a lot of effort and experimenting, but with determination I was able to set
up the runners and the CI/CD pipeline for the front-end and back-end, including code reviews using SonarQube. This
allowed me to automatically update my application and thus work more efficiently.

#### Personal Leadership

Throughout this semester I demonstrated my personal leadership by taking others with my in my own learning. Though I
pushed to expand my knowledge in areas such as with Augmented Reality, I also tried asking for help from experts when
possible and if that didn't help enough, scope it down, conclude and use what I learned as advice for future project
members that are more experienced in the area.

Just like I mentioned in Situational Orientation I shared knowledge at several layers of the project. During the
knowledge sharing sessions I shared valuable information, asked insightful questions and provided meaningful feedback to
my colleagues. I also shared the project during the XR event, IT Festival and CDT presentation.

During these moments I also asked for feedback. By taking on multiple opportunities to share knowledge and request
feedback from multiple perspectives I have shown that I am actively working on my personal and project development and
towards being an IT professional.

#### Targeted Interaction

Throughout this project I have communicated and collaborated with several partners and stakeholders. First of all there
is my company coach, Simona, who is the owner of the City Digital Twin project and the main stakeholder of the project.
During this project I had a weekly meeting with Simona, where we discussed my progress, next steps and any troubles I
was having. She had access to my repositories, and if I wanted feedback on anything I would send her an email with what
I wanted feedback on.

Next there are my colleagues at the lectorate. My fellow interns I mostly had casual conversation with and helped them
with their projects. If there was anything I wanted feedback on, I would show it in the knowledge sharing sessions so I
would be able to get feedback from both my fellow interns and long-term Fontys employees. With these long-term Fontys
employees I also had check-ins once in a while on how my project was progressing. For example Bart helped me when I was
struggling with the weight of a fine dust particle and seeing as his project at the Lectorate was also relevant I also
shared my portfolio with him every so often so he could read through it.

I also communicated with our expert, Jean-Paul Closé. After getting in contact with him through Simona via email, we set
up a lunch meeting where I got to do an interview and we discussed the project and he explained more about Fine dust.
Afterwards we had a bit more contact once the data showed irregularities.

TNO was an unexpected partner. After we found some irregularities in the data compared to the report, we contacted them.
Through mail contact they already explained a few things, but we did set up a meeting for further discussions. During
that meeting they were a bit of a second expert, but we also managed to provide insights for them about the API and how
the data could be interpreted.

Through Bart I managed to get in contact with two groups of Semester 6 media students. They were approached by Bart and
sent to my testing setup where they could give me feedback on my designs from their expert opinion.

Lastly there were the expo visitors. During this project there were a total of 4 expo's or variations thereof. There was
the poster market, which not a lot of people came to unfortunately. There was the XR Event, where I was able to interact
with some people but less than hoped despite the amount of people in attendance. The IT festival was the most successful
event, as there was a lot of interaction and feedback from a variety of perspectives. I needed to communicate what I was
doing on multiple levels but based on the feedback I managed to make this work. Lastly there was the CDT Webinar. This
event there was some interaction, but mostly with people that are working on similar areas. This, of course, is also
valuable information.

#### Personal Goals

##### Incorporate feedback from others

During this semester I tried to take any feedback opportunities that were presented to me. Not all of these succeeded
but I did manage to find a balance where I worked with people to incorporate their feedback into my application. I know
that there must still have been some rigidness from my part, but whether that was healthy scoping and boundaries or
rigidness in allowing other perspectives I cannot determine. Nonetheless I actively worked towards this goal and have
improved on this aspect.

##### Create realistic scopes

My first scope wasn't the final scope, but I did work on better adjusting the scopes during the project. Perhaps I could
have achieved my original scope if the TNO data didn't cause such a delay, but this is also part of scoping. I did
manage to follow most of my approach and due to my progress reports I also have better indications of how long I spent
on something so I could better scope in the future.

##### Ask for help on time

Whether I asked for help on time or could have asked for help sooner is still debatable, but I did ask for help in a
timely manner. On several aspects I asked for help, for the domain knowledge I got recommended the expert. On AR I got
recommended Bernd-Jan. On AI Simona helped for a bit and we got into contact with TNO. For the people aspect I asked
Bart for help and he arranged help from the media students, which in turn is how I got help with the design aspect.

I believe the longest I have waited to ask for help was for the AR aspect, as I wanted to make the theoretical knowledge
work, but due to issues with for example versioning this was simply not possible. Still, this was something that I could
have improved upon by for example setting a goal beforehand on when I should ask for help.