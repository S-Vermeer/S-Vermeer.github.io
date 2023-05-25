---
layout: page
title: Software Architecture Documentation
permalink: /documents/software-architecture-documentation
parent: Documents
grand_parent: Reference
nav_order: 4
---

# City Digital Twin AR Zoom Lens - Software Architecture Documentation
**_Fontys Lectorate High Tech Embedded Systems Eindhoven_**
_Eindhoven_

| Date    | 27-02-2023 |
|---------|-------------------|
| Version |             0.1.0 |
| Status  | Layout            |
| Author  | Skyler Vermeer    |  

<div style="page-break-after: always;"></div>

## Document History
### Version

|   **Version**            |   **Date**                      |   **Author**           |   **Amendments**                                             |   **Status**      |
|:-------------------------|:--------------------------------|:-----------------------|:-------------------------------------------------------------|:------------------|
|                  0.1.0   |                 27-02-2023      |   Skyler Vermeer       |   Layout                                                     |   Layout          |
|   1.0.0                  |   19-04-2023                    |   Skyler Vermeer       |   Update Contexts                                            |   Draft           |  

### Communication

|  **Version**  |  **Date**  |  **To**  |
|:--------------|:-----------|:---------|
|  XXX          |  xxxx      |  xxxx    |
|  XXX          |  xxxx      |  xxxx    |  

<div style="page-break-after: always;"></div>

## Introduction
The goal of this document is to depict the architecture of the application in detail, harbour the functional and non-functional requirements, and document decisions and maintenance.

The application that is described in this document is an application where you can use augmented reality to zoom in on fine dust particles and see their impact in the future if trends continue.
<div style="page-break-after: always;"></div>

## Table of Contents
- [Document History](#document-history)
	- [Version](#version)
	- [Communication](#communication)
- [Introduction](#introduction)
- [Table of Contents](#table-of-contents)
- [1. Contexts](#1-contexts)
  - [1.1 C1](#11-c1)
  - [1.1 C2](#12-c2)
  - [1.1 C3](#13-c3)
  - [1.1 C4](#14-c4)
- [2. Data Storage](#2-data-storage)
  - [2.1 Data Collection & Storage](#21-data-collection--storage)
  - [2.2 Data Choices](#22-data-choices)
- [3. CI/CD Pipeline](#3-cicd-pipeline)
  - [3.1 Continuous Integration](#31-continuous-integration)
  - [3.2 Continuous Deployment](#32-continuous-deployment)

<div style="page-break-after: always;"></div>

## 1. Contexts
### 1.1 C1
![[Pasted image 20230420160730.png]]
### 1.2 C2
![[Pasted image 20230420160714.png]]

<div style="page-break-after: always;"></div>

## 2. Data Storage
### 2.1 Data Collection & Storage
In my EDA I have extensively described the data sourcing, so I am going to be focusing here on the relevant data and how it was obtained. 

Our PM data is obtained through the ILM2 API. By scraping it, we can obtain the backlog of data, and get more specific data once we need it. An example of the link that we scraped is https://api.dustmonitoring.nl/v1/project(30)/location(575)/stream?$skip=14544. This means that for location 575 we have already obtained the first 14544 entries, and want the next set. After that we can use @odata.nextLink to get the next url. This gives entries seperated by measurements. This means that a set of measurements is given, with for example the longitude, latitude and PM10 seperately. A sample of a single location (521) gives the following.
![[Pasted image 20230516131821.png]]

After combining them into a single 'prepared' dataframe, saved as full_measurement.csv, we have a dataframe that combines the measurements into a single row. The head of this dataframe, focused on a single location, is the following.
![[Pasted image 20230516133334.png]]

As you can see, it has the three relevant measurements, their date and time, and the location they belong to in a single entry.

There is also measurement_info.csv and locaties-airboxen.csv. Two supplemental datasets. measurement_info is a reference for the units. This means that if you have the name of the entity, such as PM1, it can associate the unit to it. Seeing as the unit isn't always relevant, this was placed seperately for easy reference. The same goes for locaties-airboxen.csv. 


For the modelling, we also worked with a dataset that combined some relevant weather data with our existing measurements. However, currently the only weather data we have is until November 1st 2022, saved in hourdata_eindhoven_2021_01nov2022.csv.  
![[Pasted image 20230516135812.png]]
Originally the data we obtained  from the KNMI had much more variables, but we didn't need most of them, as they weren't relevant to our domain. The full dataset can be found as uurgeg_370_2021-2030.csv.


After combining it with our PM measurements with the frequency of an hour and using the average, we saved this in hour_combined_data_eindhoven_2021_01nov2022.csv. This indicates the timeframe, that it is for every hour, and is for Eindhoven.

![[Pasted image 20230516135247.png]]![[Pasted image 20230516135309.png]]

With these datasets we did our modelling. For our closest location model, we used locaties-airboxen.csv. This file is also found in the same folder as our API so it is included in the docker image. The other files are currently in the back-end repository but not included in the docker image, as they are added on an as needed basis. They are seperated between raw files (such as the scraped data) and non-raw files, such as the locaties-airboxen.csv. 

Within the application the process is as follows:
1. The user visits cdt.skylervermeer.nl/ar.html
2. The website requests the user's location.
3. The user's location is sent to the API and is used with a KNN model to get the closest location.
4. The closest location ID is returned and stored in a global variable.
5. The closest location data is requested in the API and returned and the models are displayed.

### 2.2 Data Choices
At the start, when choosing our data we had a few requirements:
-   The data must be collected consistently, so we can see trends and outliers.
-   The data must be collected regularly, so we can show the current amount of fine dust in the area.
-   The data must contain some form of timestamp.
-   The data must contain some form of location.
-   The data must contain a measurement of fine dust, preferably PM1, PM2.5 and PM10.

Eventually we chose to use the ILM2 network from TNO, as this is a renowned research organization. This allowed us to have data for every 10 minutes, updated every hour. It had locations, timestamps and PM1, PM2.5, and PM10 (as well as some others). This was also the data that our expert Jean-Paul worked with, so it allowed for easier communication. 

However, as we were exploring this data we found some irregularities. We contacted TNO and had a meeting about it, resulting in them saying that for the most accurate results we needed to use the placement dates as they are mentioned in their 2021 report. For example a location (575) that was placed in november of 2021, already had data for 2019 due to an error in their API. 

So the data is more accurate than regular sensor networks, easily accessible with an API and constantly monitored for downtime by a renowned organisation. However, the API in itself isn't trustworthy and some data needs to be verified. 

For the weather data we used the data from KNMI, which had 2021 to November 2022 data. It has hourly data for Eindhoven, but seeing as we also need the data for  November 2022 to now, we do need something supplemental for deployment. We can use some type of forecasting API for our own AI, but until that time we do need data to train on. However, for now it allowed us to test the model without paying from an account from an API, without having to scrape ourselves and spend a lot of time there. 

## 3. Repositories and CI/CD Pipeline
### 3.1 Repositories
Originally, I had a single repository with the front-end, back-end and documentation of the project, [Combined Repository](https://git.fhict.nl/I428513/Digital-Twin-City---AR-Zoom-Lens). It seemed a good idea at first, but it meant that I couldn't switch branches without losing progress, and if I had documentation in two different experimentation branches it would cause problems. That is why, when I was able, I switched it to three seperate repositories. These repositories are [Front-end](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-front-end), [Back-end](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-back-end), and [Documentation](https://git.fhict.nl/I428513/cdt-a-i-r-zoom-lens-documentation). Front-end and Back-end both have CI/CD pipelines that automatically update the docker images and renew the containers hosting the application on a commit, and SonarQube to get automatic code reviews. Documentation does not need that functionality.

### 3.2 Continuous Integration
The main reason that we are implementing Continuous Integration is so code is evaluated automatically so large errors are caught early. I have worked with SonarQube before and I have worked with Gitlab pipelines before so I know both of these work. That is why I will be using these to set up my pipeline, at least in the basics.

We are using a docker-in-docker image to update the docker image for continuous integration. The publish step will build the new image, login to docker using hidden variables and push to the relevant docker tag.
```
  - docker build ./Website -t skylervermeer/cdt-air-zoomlens:website  
  - docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD  
  - docker push skylervermeer/cdt-air-zoomlens:website
```

After that we are processing it through SonarQube. Currently, it will allow it to fail. However, this can be adjusted so if the Quality Check does not succeed, the pipeline will fail. For this we use the sonar-scanner-cli image, and the sonar-scanner command. Based on some hidden variables this will automatically scan and show the results at the [dashboard](http://sonarqube.skylervermeer.nl/projects).

### 3.3 Continuous Deployment
After a successfull integration, we want to deploy the new application to our own environment and domain. This is useful because the application requires HTTPS and CORS in some aspects, so this automates the deployment on a commit. You can also adjust this so deployment only executes when the commit is pushed to a certain branch, but for our development purposes this is not worth it.

For this deployment we use python, as we want to connect to a SSH (our server) to deploy our application.
```
- chmod og= $ID_RSA  
- apk update && apk add openssh-client  
- ssh -i $ID_RSA -o StrictHostKeyChecking=no $SERVER_USER@$SERVER_IP "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"  
- ssh -i $ID_RSA -o StrictHostKeyChecking=no $SERVER_USER@$SERVER_IP "docker pull skylervermeer/cdt-air-zoomlens:website"  
- ssh -i $ID_RSA -o StrictHostKeyChecking=no $SERVER_USER@$SERVER_IP "docker container rm -f cdt-frontend || true"  
- ssh -i $ID_RSA -o StrictHostKeyChecking=no $SERVER_USER@$SERVER_IP "docker run -d -p 8000:8000 --name cdt-frontend skylervermeer/cdt-air-zoomlens:website"
```
This will login to docker, pull the new image, remove the previous image and run the updated image on the port that is forwarded to cdt.skylervermeer.nl.

<div style="page-break-after: always;"></div>
## Conclusion
xxxx

<div style="page-break-after: always;"></div>
## Sources
xxx