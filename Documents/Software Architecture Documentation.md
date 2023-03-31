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

|  **Version**          |  **Date**              |  **Author**          |  **Amendments**                                            |  **Status**  |
|:----------------------|:-----------------------|:---------------------|:-----------------------------------------------------------|:-------------|
|                0.1.0  |  27-02-2023     |  Skyler Vermeer      |  Layout  |  Layout         |
|  xxxx                 |  xxx                   |  xxxx                |  xxx                                                       |  xxx         |  

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
System context - Abstract Flow

### 1.2 C2
Containers and Tech choices (Ex. microservices approach)

### 1.3 C3
Components incl interfaces etc

### 1.4 C4
Communication Components

<div style="page-break-after: always;"></div>

## 2. Data Storage
### 2.1 Data Collection & Storage
{Architecture? API? Dataframes overview?}
xxxx

### 2.2 Data Choices
What choices were made what are the benefits and downsides

## 3. CI/CD Pipeline
### 3.1 Continuous Integration
The main reason that we are implementing Continuous Integration is so code is evaluated automatically so large errors are caught early and some basic automatic testing can be applied. I have worked with SonarCloud before and I have worked with Gitlab pipelines before so I know both of these work. That is why I will be using these to set up my pipeline, at least in the basics. If more requirements come up later this might be adjusted, however for now the goal is to have something that works, and have it be low effort in setup.

### 3.2 Continuous Deployment
Continuous deployment is relevant so after a successful deployment the application is automatically deployed to the production environment.

<div style="page-break-after: always;"></div>
## Conclusion
xxxx

<div style="page-break-after: always;"></div>
## Sources
xxx