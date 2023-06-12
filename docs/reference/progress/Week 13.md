---
layout: page
title: Week 13
permalink: /progress/week13
parent: Progress
grand_parent: Reference
nav_order: 13
---
# Progress Report - Week 13

### General info
**Week**: 13
**Phase**: 3 - Component Development  
**Contacted**: Meeting with Simona
**Document(s) handed in**: {XXX}  

##### Biggest accomplishment
Adding weather data to the models so there actually is something that works

##### Biggest setback
KNMI was not updated (yet)

### Daily progress
![[Week13-progress.png]]

##### Monday
On Monday I worked on getting the weather data for using polynomial features to see whether that would produce a better model. I also tried out predicting the next 12 hours to see if that would produce better models. At first I wanted to look for an API for weather data, but then I remembered that in a previous project we also used hourly weather data for Eindhoven, so I found the updated version of that from the KNMI and worked on selecting and combining the relevant data of that so I could test it before working on combining it with an API, and perhaps even paying for an account for something that doesn't even work. I also updated the Prophet model to include the weather data as that model also allowed for it.

##### Tuesday
On Tuesday I moved the weather data a bit up so it could be reached by the other chapters as well, which took some time. I also mainly worked on my Software Architecture document, the Data Storage and Choices, and the CI/CD pipeline and repositories.

##### Wednesday
Work on portfolio conversion to website Github Pages, using just-the-docs theme and creating to do list for updating portfolio

##### Thursday, Friday + Weekend
Ascension Day + Free day after ascension day. Mainly scraping data from last time of scraping till now, finding a weather API to use and find a way to make the API update the data automatically... through lots of troubleshooting.
