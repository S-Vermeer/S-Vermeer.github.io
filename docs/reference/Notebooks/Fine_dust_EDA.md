---
layout: page
title: Fine_dust_EDA
permalink: /notebooks/Fine_dust_EDA/
parent: Notebooks
grand_parent: Reference
nav_order: 1
---

# Data Collection and Preparation - City Digital Twin AR Zoom lens
##### Skyler Vermeer - Intern Lectorate HTES
Version 1.3.3, March 21st 2023

## Version Control

| **Version** | **Date**                        | **Author** | **Amendments**                                                               | **Status**                              |
|:------------|:--------------------------------|:-----------|:-----------------------------------------------------------------------------|:----------------------------------------|
| 1.0.0       | February 16th 2023              | Skyler Vermeer | Layout + test out downloaded files data                                      | Draft                                   |
| 1.1.0 - 9   | February 17th - March 13th 2023 | Skyler Vermeer | Various variations of code to scrape data from the API                       | Draft                                   |
| 1.2.0       | March 3rd 2023                  | Skyler Vermeer | Exploring 1 location (1 day)                                                 | Draft - old version still being updated |
| 1.3.0       | March 14th 2023                 | Skyler Vermeer | Combining all data into full datasets                                        | Draft                                   |
| 1.3.1       | March 15th 2023                 | Skyler Vermeer | Extend documentation (Sourcing, layout requirement)                          | Draft                                   |
| 1.3.2       | March 16th 2023                 | Skyler Vermeer | Verfication of start dates and units, overall layout and explanation update. | Draft                                   |
| 1.3.3       | March 21st 2023                 | Skyler Vermeer | Requirements, Data Quality                                                   | Draft                                   |
| 1.3.4       | March 28th 2023                 | Skyler Vermeer | Exploring Single day                                                         | Draft                                   |
| 1.3.5       | March 30th 2023                 | Skyler Vermeer | Exploring Single day - continued                                             | Draft                                   |



The first thing we do with any exploratory data analysis, is get our basic packages imported and print their versions so it is transparent what we did and can be reproduced.

```python
import os
from datetime import datetime

import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

%matplotlib inline

print('pandas version:', pd.__version__)
print('numpy version:', np.__version__)
print('matplotlib version:', matplotlib.__version__)
```

    pandas version: 1.5.2
    numpy version: 1.23.0
    matplotlib version: 3.6.2

## Data Sourcing
To obtain data that is relevant to my Fine Dust domain and can help with anwering my subquestions: "What data is available regarding this problem?" and "How can AI be used to make predictions using this data?", I went to the Open Eindhoven Data [Eindhoven Open Data](https://data.eindhoven.nl/pages/home/). I did this, because the City Digital Twin project works on the basis of creating a Digital Twin of the City Eindhoven using the open data that the city shares. Here I searched for all the datasets relevant to fine dust.

Different datasets based on fine dust on platform.
1. Real-time fine dust monitoring {SOURCE}, it contains real-time data of fine dust in Eindhoven, updated every hour and collected every 10 minutes. It is collected over just under 50 locations in and around the city Eindhoven. It is made to give an overview of the air quality in South-East Brabant. It measures PM1, PM2.5, PM10 and NO2. It has locations of varying concentrations of fine dust particles. They give you the ability to download the data, select specific locations and download in batches of up to 70 days. It also has an API {SOURCE} where you can obtain historical data, or search for specific moments, and filter what features you want.
2. Locaties Caireboxen Fijnstof-monitoring {SOURCE}, it contains the locations of the Fine Dust monitoring boxes, in the Eindhoven municipality. It has the ID, name, town and street, observables, API URL and location of the monitoring boxes.

During my research I was referred to some datasets for additional information or data
1. SamenMeten {SOURCE}, contains data collected by a variety of organizations and is combined by RIVM into one data portal. Most sensors measure multiple times an hour, and air quality is averaged each hour. The measurements are calibrated with the use of data science, to correct unwanted influences such as humidity. Raw sensormeasurements are also available.
2. WorldClim {SOURCE}, contains climate data for 1970-2000. There is monthly climate data for various weather features, as well as 19 bioclimatic variables. It is available at 4 spatial resulations, between 30 seconds to 10 minutes. There is also monthly weather data from 1960-2018 regarding the minimum and maximum temperature and precipation. Based on this, a model was created for future weather and climate predictions.

Based on the research, and the data we have available, we are going to visualize the current fine dust data and predict the amount of fine dust (PM1, PM2.5 and PM10) in the future based on historical data and if possible also incorporating weather and climate. This, because it fits the requests of the client, helps with answering the research question and the data allows for it. The weather data is added as based on my domain research, the weather is an important aspect in the behaviour of fine dust, so if possible we want to incorporate this. There are other prediction opportunities, such as classifying the type of fine dust, whether it is harmful or not, however that is not possible within the current data.

## Data requirements

For this project we want to create a prediction of the amount of fine dust in the air at a certain moment in the future. We also want to show the current amount of fine dust in the area. To do this, we will need a few things from our data.

- The data must be collected consistently, so we can see trends and outliers.
- The data must be collected regularly, so we can show the current amount of fine dust in the area.
- The data must contain some form of timestamp.
- The data must contain some form of location.
- The data must contain a measurement of fine dust, preferably PM1, PM2.5 and PM10.

Based on this, we will be using the real-time fine dust monitoring that was shared on the Eindhoven Open Data platform. We will use the Locaties Caireboxen fijnstof monitoring to verify the locations that are being monitored.

If we want, we could use WorldClim to incorporate weather and climate data into our prediction. However, for now we will first explore the fine dust monitoring dataset and work from there.

## Collecting the Data

### Downloading
To start this project, I manually downloaded data from December 21st to February 16th 2023 due to the 70 day maximum. I stored the manually downloaded files in a directory called csv_files/manual_download for ease of access.

```python
manual_file_path = "C:/Users/Public/Documents/Github/Internship-S5/cdt-a-i-r-zoom-lens-back-end/csv_files/manual_download/"
```

Let's take a look at what this data looks like

```python
pd.read_csv(manual_file_path + 'Dustmonitoring data.2022-12-21.2023-02-16.csv', sep=';')
```

    C:\Users\micro\AppData\Local\Temp\ipykernel_31084\441551080.py:1: DtypeWarning: Columns (7,13,14,20,21,22,23,24,25,26,32,33,34,35,36,37,38,44,50,56,62,63,64,65,66,67,68,74,80,81,87,93,99,105,111,117,123,129,130,136,142,148,154,160,166,172,178,184,190,196,202,208,214,220,226,232,238,244,250,251,252,253,254,255,256,262,268,274,280,286) have mixed types. Specify dtype option on import or set low_memory=False.
      pd.read_csv(manual_file_path + 'Dustmonitoring data.2022-12-21.2023-02-16.csv', sep=';')



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Unnamed: 1</th>
      <th>I01</th>
      <th>I01.1</th>
      <th>I01.2</th>
      <th>I01.3</th>
      <th>I01.4</th>
      <th>I01.5</th>
      <th>I02</th>
      <th>I02.1</th>
      <th>...</th>
      <th>I57.2</th>
      <th>I57.3</th>
      <th>I57.4</th>
      <th>I57.5</th>
      <th>I58</th>
      <th>I58.1</th>
      <th>I58.2</th>
      <th>I58.3</th>
      <th>I58.4</th>
      <th>I58.5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Time</td>
      <td>Time</td>
      <td>Lat</td>
      <td>Lon</td>
      <td>PM1</td>
      <td>PM2.5</td>
      <td>PM10</td>
      <td>NO2</td>
      <td>Lat</td>
      <td>Lon</td>
      <td>...</td>
      <td>PM1</td>
      <td>PM2.5</td>
      <td>PM10</td>
      <td>NO2</td>
      <td>Lat</td>
      <td>Lon</td>
      <td>PM1</td>
      <td>PM2.5</td>
      <td>PM10</td>
      <td>NO2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>UTC</td>
      <td>Local</td>
      <td>°</td>
      <td>°</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³\r\n</td>
      <td>°</td>
      <td>°</td>
      <td>...</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³\r\n</td>
      <td>°</td>
      <td>°</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³\r\n</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-12-21 00:00:00</td>
      <td>2022-12-21 01:00:00</td>
      <td>51,4063</td>
      <td>5,4574</td>
      <td>17,21</td>
      <td>19,13</td>
      <td>30,32</td>
      <td>22</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>10,44</td>
      <td>10,71</td>
      <td>20,22</td>
      <td>11</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-12-21 00:00:01</td>
      <td>2022-12-21 01:00:01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51,4379</td>
      <td>5,3581</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-12-21 00:00:04</td>
      <td>2022-12-21 01:00:04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51,4904</td>
      <td>5,3941</td>
      <td>15,75</td>
      <td>16,54</td>
      <td>17,94</td>
      <td>15</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>28915</th>
      <td>2023-02-16 23:40:04</td>
      <td>2023-02-17 00:40:04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51,4904</td>
      <td>5,3941</td>
      <td>11,01</td>
      <td>11,85</td>
      <td>13,49</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>28916</th>
      <td>2023-02-16 23:49:59</td>
      <td>2023-02-17 00:49:59</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28917</th>
      <td>2023-02-16 23:50:00</td>
      <td>2023-02-17 00:50:00</td>
      <td>51,4063</td>
      <td>5,4573</td>
      <td>15,00</td>
      <td>16,16</td>
      <td>17,20</td>
      <td>2.0</td>
      <td>51,4379</td>
      <td>5,3582</td>
      <td>...</td>
      <td>9,80</td>
      <td>10,20</td>
      <td>24,09</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28918</th>
      <td>2023-02-16 23:50:01</td>
      <td>2023-02-17 00:50:01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28919</th>
      <td>2023-02-16 23:50:04</td>
      <td>2023-02-17 00:50:04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51,4904</td>
      <td>5,3941</td>
      <td>10,48</td>
      <td>11,02</td>
      <td>11,56</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
<p>28920 rows × 287 columns</p>
</div>


We can see that the information is quite wonky. It is technically the information, but it has a column for every location and measurement. The columns that are not related to the location of the entry, are NaN. It would need a lot of adjusting to gather the info, not really a good idea. Let's look at a selection of a single location

```python
pd.read_csv(manual_file_path + 'Dustmonitoring data.2022-12-21.2023-02-16_I01.csv', sep=';')
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Unnamed: 1</th>
      <th>I01</th>
      <th>I01.1</th>
      <th>I01.2</th>
      <th>I01.3</th>
      <th>I01.4</th>
      <th>I01.5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Time</td>
      <td>Time</td>
      <td>Lat</td>
      <td>Lon</td>
      <td>PM1</td>
      <td>PM2.5</td>
      <td>PM10</td>
      <td>NO2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>UTC</td>
      <td>Local</td>
      <td>°</td>
      <td>°</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³</td>
      <td>μg/m³\r\n</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-12-21 00:00:00</td>
      <td>2022-12-21 01:00:00</td>
      <td>51,4063</td>
      <td>5,4574</td>
      <td>17,21</td>
      <td>19,13</td>
      <td>30,32</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-12-21 00:10:00</td>
      <td>2022-12-21 01:10:00</td>
      <td>51,4063</td>
      <td>5,4574</td>
      <td>18,29</td>
      <td>20,75</td>
      <td>25,83</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-12-21 00:20:01</td>
      <td>2022-12-21 01:20:01</td>
      <td>51,4063</td>
      <td>5,4574</td>
      <td>18,88</td>
      <td>22,04</td>
      <td>32,27</td>
      <td>25</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8340</th>
      <td>2023-02-16 23:10:00</td>
      <td>2023-02-17 00:10:00</td>
      <td>51,4063</td>
      <td>5,4573</td>
      <td>19,13</td>
      <td>21,14</td>
      <td>29,29</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8341</th>
      <td>2023-02-16 23:20:00</td>
      <td>2023-02-17 00:20:00</td>
      <td>51,4063</td>
      <td>5,4573</td>
      <td>18,37</td>
      <td>20,23</td>
      <td>32,35</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8342</th>
      <td>2023-02-16 23:30:00</td>
      <td>2023-02-17 00:30:00</td>
      <td>51,4063</td>
      <td>5,4573</td>
      <td>17,36</td>
      <td>19,13</td>
      <td>26,48</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8343</th>
      <td>2023-02-16 23:40:01</td>
      <td>2023-02-17 00:40:01</td>
      <td>51,4063</td>
      <td>5,4573</td>
      <td>18,19</td>
      <td>19,54</td>
      <td>25,84</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8344</th>
      <td>2023-02-16 23:50:00</td>
      <td>2023-02-17 00:50:00</td>
      <td>51,4063</td>
      <td>5,4573</td>
      <td>15,00</td>
      <td>16,16</td>
      <td>17,20</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>8345 rows × 8 columns</p>
</div>


This would work as it just needs adjustment of the top 2 rows into the column name. However, manually downloading it would take too much work because 58 locations in batches of 70 days would be around 580+ requests. There is also an API. The api works with odata, which if you use the correct query will also provide you with @odata.nextlink which will give you the query to get even more of the same query aside from the data you have already gotten. This means that if we can manipulate this query to automatically process all the information, we can scrape the data in a loop (and also in the future starting from a specific point)

https://api.dustmonitoring.nl/v1/project(30)/location(521)/stream I found that this gives the data for location '521', and gives an @odata.nextlink.

### Using the API
So first I am going to get all the available locations.

```python
import requests as request

res = request.get("https://api.dustmonitoring.nl/v1/project(30)/locations")
data = res.json()

locations = []
for item in data['value']:
    locations.append(item['ID'])
```

Then it is time to obtain the info from all these locations.

```python
print(locations)
print(len(locations))
```

    [531, 521, 542, 543, 553, 544, 545, 532, 533, 554, 534, 535, 546, 536, 556, 522, 557, 547, 549, 524, 537, 525, 526, 539, 551, 540, 558, 527, 528, 529, 530, 560, 561, 562, 563, 564, 565, 566, 567, 568, 569, 570, 571, 572, 573, 574, 575, 576, 577]
    49

```python
file_path = "C:/Users/Public/Documents/Github/Internship-S5/cdt-a-i-r-zoom-lens-back-end/csv_files/raw_data/"
file_list = os.listdir(file_path)
```

```python
for location in locations:
    request_string = "https://api.dustmonitoring.nl/v1/project(30)/location(" + str(location) + ")/stream"
    index = 0
    dataframes = []
    while index != -1:
        res = request.get(request_string)
        data = res.json()
        df = pd.DataFrame(data['value'])
        dataframes.append(df)
        try:
            request_string = data['@odata.nextLink']
        except KeyError:
            index = -1
        print(request_string)
    combined_df = pd.DataFrame()
    for dataframe in dataframes:
        combined_df = combined_df.append(dataframe)

    combined_df.to_csv(file_path + str(location) + '_raw_data')
print("done")

```

```python
# This code block is to save a location if the last step (saving) threw an error for whatever reason

# combined_df.to_csv(file_path + str(577) + '_raw_data')
```

```python
# This code block is to finish a location if it was interrupted while scraping so it doesn't have to go from the start.

# while index != -1:
#     res = request.get(request_string)
#     data = res.json()
#     df = pd.DataFrame(data['value'])
#     dataframes.append(df)
#     try:
#         request_string = data['@odata.nextLink']
#     except KeyError:
#         index = -1
#     print(request_string)
# combined_df = pd.DataFrame()
# for dataframe in dataframes:
#     combined_df = combined_df.append(dataframe)
#
# combined_df.to_csv(file_path + str(location) + '_raw_data')
```

The next piece of code is to update the dataframe since the initial scraping, so it can be combined with KNMI data. It is practically the same, but it updates from the last known data point.

```python
current_data = pd.read_csv(file_path + str(531) + "_raw_data")
current_data.tail(1).Timestamp.values[0]
```




    '2023-05-19T09:00:00Z'



```python
for location in locations:
    current_data = pd.read_csv(file_path + str(location) + "_raw_data")
    last_datetime = current_data.tail(1).Timestamp.values[0]
    request_string = "https://api.dustmonitoring.nl/v1/project(30)/location(" + str(location) + ")/stream?$filter=timestamp gt " + last_datetime
    index = 0
    dataframes = []
    while index != -1:
        res = request.get(request_string)
        data = res.json()
        df = pd.DataFrame(data['value'])
        dataframes.append(df)
        try:
            request_string = data['@odata.nextLink']
        except KeyError:
            index = -1
        print(request_string)
    combined_df = current_data
    for dataframe in dataframes:
        combined_df = pd.concat([combined_df, dataframe], ignore_index=True)

    combined_df.to_csv(file_path + str(location) + '_raw_data')
print("done")

```

## Preparation

### Single location
Now that we have all the data, it is time to combine the raw data into a large dataset. But first, let's look at a single location dataset.

```python
df_521 = pd.read_csv(file_path + '521_raw_data')
print(df_521.sample(5))
```

            Unnamed: 0.4  Unnamed: 0.3  Unnamed: 0.2  Unnamed: 0.1  Unnamed: 0  \
    397655        397655      397655.0      397655.0      397655.0        71.0   
    547340        547340      547340.0      547340.0      547340.0       140.0   
    559577        559577      559577.0      559577.0      559577.0       137.0   
    574935        574935      574935.0      574935.0      574935.0        87.0   
    134920        134920      134920.0      134920.0      134920.0       136.0   
    
                       Timestamp  Location  Entity  Name    Value       Unit  
    397655  2022-08-03T01:10:00Z       521     146   NO2   9.0000  μg/m³\r\n  
    547340  2023-01-07T11:00:00Z       521      10   Lat  51.4379          °  
    559577  2023-01-19T15:20:01Z       521     146   NO2   8.0000  μg/m³\r\n  
    574935  2023-02-04T22:40:01Z       521     130  PM10  43.5100      μg/m³  
    134920  2021-08-20T10:30:01Z       521      11   Lon   5.3888          °  

We see we start with an unnamed column, which is because the dataframes were combined including their index, so we need to remove that. It would also be useful if we seperate the timestamp into date and time. It might be useful to combine the name and unit as that is just useful for reference. Or create a separate dataframe to reference it. It is also useful to seperate the Lon/Lat values from the rest of the values as even though the values are there, and we need them, they are somewhat the same per location with a few inaccuracies in measuring. So we should save the locations in a separate csv, and make it one per location. Entity is the ID associated with a measurement, so all Lat measurements have entity 10 and all PM10 have entity 130. Also in units some of them have \r\n attached to them, which isn't part of the unit.

First, let's remove the extra, unnamed index column.

```python
print(df_521.loc[:, 'Timestamp':'Unit'].sample(5))
```

                       Timestamp  Location  Entity   Name  Value       Unit
    119760  2021-07-30T07:00:00Z       521     129  PM2.5   8.91      μg/m³
    300905  2022-03-15T19:10:00Z       521     130   PM10  19.96      μg/m³
    391674  2022-07-26T22:10:00Z       521     129  PM2.5   2.69      μg/m³
    26772   2021-03-24T02:00:00Z       521     146    NO2  13.00  μg/m³\r\n
    468343  2022-10-20T10:40:00Z       521     146    NO2  10.00  μg/m³\r\n

Now, let's separate the values. The names are short enough that they can easily be used as an index. So we want to split the data into three datasets. A dataset with the different locations (ID combined with Lat and Lon), a dataset with the different measurements (Entity ID, Name, and Unit), and a dataset with the main data we will be using, which will be a separate column for day and time, a location id, a name , and a measurement.

Let's check first whether there is one unique unit per measurement or whether there are multiple.

```python
df_521.groupby('Name')['Unit'].unique()
```




    Name
    Lat                   [°]
    Lon                   [°]
    NO2           [μg/m³\r\n]
    PM1               [μg/m³]
    PM10              [μg/m³]
    PM2.5             [μg/m³]
    UFP      [counts/cm³\r\n]
    Name: Unit, dtype: object



It doesn't look like it. Of course this is just one location, and we need to check with the others, but it is positive that at least for this location it is just a static combination. Let's just save all this as a static dataframe for now, and we can verify whether it is the same with all locations later.

```python
units = df_521.groupby('Name')['Unit'].unique()
entities = df_521.groupby('Name')['Entity'].unique()
measurement_info = pd.DataFrame()
units[2][0] = units[2][0][:-2]
units[6][0] = units[6][0][:-2]

for idx in range(len(units)):
    measurement_info = pd.concat(
        [measurement_info, pd.DataFrame([units.index[idx], units[idx][0], entities[idx][0]]).T])
measurement_info.columns = ['Name', 'Unit', 'Entity_ID']
measurement_info = measurement_info.reset_index(drop=True)
print(measurement_info)
```

        Name        Unit Entity_ID
    0    Lat           °        10
    1    Lon           °        11
    2    NO2       μg/m³       146
    3    PM1       μg/m³       128
    4   PM10       μg/m³       130
    5  PM2.5       μg/m³       129
    6    UFP  counts/cm³       145

```python
measurement_info.to_csv(
    'C:/Users/Public/Documents/Github/Internship-S5/cdt-a-i-r-zoom-lens-back-end/csv_files/' + 'measurement_info',
    index=False)
```

Now, let's see about the locations. We know that though the there is only one correct longitude and latitude combination per location, due to some errors there are sometimes other locations. However, there is also the dataset that is saved in 'locaties-airboxen.csv' which contains the actual locations of the sensor.

```python
airboxes = pd.read_csv('csv_files/locaties-airboxen.csv', sep=';')
airbox_521 = airboxes.loc[airboxes.ID == 521]
airbox_521
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Name</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Description</th>
      <th>Observables/0</th>
      <th>Observables/1</th>
      <th>Observables/2</th>
      <th>Observables/3</th>
      <th>Observables/4</th>
      <th>URL</th>
      <th>geopoint</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12</th>
      <td>521</td>
      <td>I02</td>
      <td>51.437895</td>
      <td>5.358142</td>
      <td>Eindhoven, Scherpenering</td>
      <td>Lat</td>
      <td>Lon</td>
      <td>PM1</td>
      <td>PM2.5</td>
      <td>PM10</td>
      <td>https://api.dustmonitoring.nl/v1/project(30)/l...</td>
      <td>51.437895, 5.358142</td>
    </tr>
  </tbody>
</table>
</div>


```python
lon = airbox_521['Lon']
lat = airbox_521['Lat']
print(str(521) + ', Lat: ' + str(lat.values[0]) + ', Lon: ' + str(lon.values[0]))
```

    521, Lat: 51.437895, Lon: 5.358142

Now we know how to easily get more data about the different locations. The last and most important aspect that we need to get is the data that we want to use. For that we want a separate column for day and time, a location id, a name , and a measurement.

```python
measurement_521_data = df_521.loc[:, ['Timestamp', 'Location', 'Name', 'Value']]
measurement_521_drop_unused = measurement_521_data[
    (measurement_521_data.Name != 'Lat') & (measurement_521_data.Name != 'Lon') & (
            measurement_521_data.Name != 'NO2') & (measurement_521_data.Name != 'UFP')].copy()

print(measurement_521_drop_unused)
```

                       Timestamp  Location   Name  Value
    54      2021-01-21T13:00:00Z       521   PM10  22.03
    55      2021-01-21T13:00:00Z       521  PM2.5  10.54
    56      2021-01-21T13:00:00Z       521    PM1   7.29
    59      2021-01-21T13:10:00Z       521   PM10  20.90
    60      2021-01-21T13:10:00Z       521  PM2.5  10.52
    ...                      ...       ...    ...    ...
    670192  2023-05-19T08:50:00Z       521  PM2.5  13.44
    670193  2023-05-19T08:50:00Z       521   PM10  19.53
    670198  2023-05-19T09:00:00Z       521    PM1   8.15
    670199  2023-05-19T09:00:00Z       521  PM2.5  11.33
    670200  2023-05-19T09:00:00Z       521   PM10  26.28
    
    [335613 rows x 4 columns]

```python
measurement_521_drop_unused['Timestamp'] = pd.to_datetime(measurement_521_drop_unused['Timestamp'])
measurement_521_drop_unused['Date'] = [d.date() for d in measurement_521_drop_unused['Timestamp']]
measurement_521_drop_unused['Time'] = [d.time() for d in measurement_521_drop_unused['Timestamp']]
```

```python
measurement_521_drop_unused.head(5)
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timestamp</th>
      <th>Location</th>
      <th>Name</th>
      <th>Value</th>
      <th>Date</th>
      <th>Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>54</th>
      <td>2021-01-21 13:00:00+00:00</td>
      <td>521</td>
      <td>PM10</td>
      <td>22.03</td>
      <td>2021-01-21</td>
      <td>13:00:00</td>
    </tr>
    <tr>
      <th>55</th>
      <td>2021-01-21 13:00:00+00:00</td>
      <td>521</td>
      <td>PM2.5</td>
      <td>10.54</td>
      <td>2021-01-21</td>
      <td>13:00:00</td>
    </tr>
    <tr>
      <th>56</th>
      <td>2021-01-21 13:00:00+00:00</td>
      <td>521</td>
      <td>PM1</td>
      <td>7.29</td>
      <td>2021-01-21</td>
      <td>13:00:00</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2021-01-21 13:10:00+00:00</td>
      <td>521</td>
      <td>PM10</td>
      <td>20.90</td>
      <td>2021-01-21</td>
      <td>13:10:00</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2021-01-21 13:10:00+00:00</td>
      <td>521</td>
      <td>PM2.5</td>
      <td>10.52</td>
      <td>2021-01-21</td>
      <td>13:10:00</td>
    </tr>
  </tbody>
</table>
</div>


Now we have successfully split the Date and Time column, it is time to start converting the data to have all the different values in one single column.

```python
df_521_measurements = pd.DataFrame(columns=['Date', 'Time', 'Location', 'PM10', 'PM2.5', 'PM1'])
```

```python
date_time_groups = measurement_521_drop_unused.groupby(['Date', 'Time'])
```

```python
[date_time_groups.get_group(x) for x in date_time_groups.groups]
```




    [                   Timestamp  Location   Name  Value        Date      Time
     54 2021-01-21 13:00:00+00:00       521   PM10  22.03  2021-01-21  13:00:00
     55 2021-01-21 13:00:00+00:00       521  PM2.5  10.54  2021-01-21  13:00:00
     56 2021-01-21 13:00:00+00:00       521    PM1   7.29  2021-01-21  13:00:00,
                        Timestamp  Location   Name  Value        Date      Time
     59 2021-01-21 13:10:00+00:00       521   PM10  20.90  2021-01-21  13:10:00
     60 2021-01-21 13:10:00+00:00       521  PM2.5  10.52  2021-01-21  13:10:00
     61 2021-01-21 13:10:00+00:00       521    PM1   7.02  2021-01-21  13:10:00,
                        Timestamp  Location   Name  Value        Date      Time
     64 2021-01-21 13:20:00+00:00       521   PM10  21.30  2021-01-21  13:20:00
     65 2021-01-21 13:20:00+00:00       521  PM2.5  10.93  2021-01-21  13:20:00
     66 2021-01-21 13:20:00+00:00       521    PM1   6.66  2021-01-21  13:20:00,
                        Timestamp  Location   Name  Value        Date      Time
     69 2021-01-21 13:30:00+00:00       521   PM10  20.23  2021-01-21  13:30:00
     70 2021-01-21 13:30:00+00:00       521  PM2.5  11.72  2021-01-21  13:30:00
     71 2021-01-21 13:30:00+00:00       521    PM1   7.67  2021-01-21  13:30:00,
                        Timestamp  Location   Name  Value        Date      Time
     74 2021-01-21 13:40:00+00:00       521   PM10  19.25  2021-01-21  13:40:00
     75 2021-01-21 13:40:00+00:00       521  PM2.5  11.37  2021-01-21  13:40:00
     76 2021-01-21 13:40:00+00:00       521    PM1   6.90  2021-01-21  13:40:00,
                        Timestamp  Location   Name  Value        Date      Time
     80 2021-01-21 13:50:00+00:00       521   PM10  21.90  2021-01-21  13:50:00
     81 2021-01-21 13:50:00+00:00       521  PM2.5  10.82  2021-01-21  13:50:00
     82 2021-01-21 13:50:00+00:00       521    PM1   7.60  2021-01-21  13:50:00,
                        Timestamp  Location   Name  Value        Date      Time
     86 2021-01-21 14:00:00+00:00       521   PM10  25.39  2021-01-21  14:00:00
     87 2021-01-21 14:00:00+00:00       521  PM2.5  11.69  2021-01-21  14:00:00
     88 2021-01-21 14:00:00+00:00       521    PM1   7.76  2021-01-21  14:00:00,
                        Timestamp  Location   Name  Value        Date      Time
     92 2021-01-21 14:10:00+00:00       521   PM10  19.76  2021-01-21  14:10:00
     93 2021-01-21 14:10:00+00:00       521  PM2.5   9.14  2021-01-21  14:10:00
     94 2021-01-21 14:10:00+00:00       521    PM1   6.13  2021-01-21  14:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     98  2021-01-21 14:20:00+00:00       521   PM10  23.09  2021-01-21  14:20:00
     99  2021-01-21 14:20:00+00:00       521  PM2.5   8.98  2021-01-21  14:20:00
     100 2021-01-21 14:20:00+00:00       521    PM1   6.74  2021-01-21  14:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     104 2021-01-21 14:30:00+00:00       521   PM10  18.02  2021-01-21  14:30:00
     105 2021-01-21 14:30:00+00:00       521  PM2.5   8.84  2021-01-21  14:30:00
     106 2021-01-21 14:30:00+00:00       521    PM1   5.82  2021-01-21  14:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     110 2021-01-21 14:40:01+00:00       521   PM10  19.40  2021-01-21  14:40:01
     111 2021-01-21 14:40:01+00:00       521  PM2.5   9.77  2021-01-21  14:40:01
     112 2021-01-21 14:40:01+00:00       521    PM1   6.74  2021-01-21  14:40:01,
                         Timestamp  Location   Name  Value        Date      Time
     116 2021-01-21 14:50:00+00:00       521   PM10  21.42  2021-01-21  14:50:00
     117 2021-01-21 14:50:00+00:00       521  PM2.5   8.81  2021-01-21  14:50:00
     118 2021-01-21 14:50:00+00:00       521    PM1   6.39  2021-01-21  14:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     122 2021-01-21 15:00:00+00:00       521   PM10  19.40  2021-01-21  15:00:00
     123 2021-01-21 15:00:00+00:00       521  PM2.5  10.25  2021-01-21  15:00:00
     124 2021-01-21 15:00:00+00:00       521    PM1   6.75  2021-01-21  15:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     128 2021-01-21 15:10:00+00:00       521   PM10  16.01  2021-01-21  15:10:00
     129 2021-01-21 15:10:00+00:00       521  PM2.5  10.04  2021-01-21  15:10:00
     130 2021-01-21 15:10:00+00:00       521    PM1   6.33  2021-01-21  15:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     134 2021-01-21 15:20:00+00:00       521   PM10  18.59  2021-01-21  15:20:00
     135 2021-01-21 15:20:00+00:00       521  PM2.5   9.15  2021-01-21  15:20:00
     136 2021-01-21 15:20:00+00:00       521    PM1   6.56  2021-01-21  15:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     140 2021-01-21 15:30:00+00:00       521   PM10  20.98  2021-01-21  15:30:00
     141 2021-01-21 15:30:00+00:00       521  PM2.5   9.07  2021-01-21  15:30:00
     142 2021-01-21 15:30:00+00:00       521    PM1   5.48  2021-01-21  15:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     147 2021-01-21 15:40:00+00:00       521    PM1   5.90  2021-01-21  15:40:00
     148 2021-01-21 15:40:00+00:00       521  PM2.5   8.33  2021-01-21  15:40:00
     149 2021-01-21 15:40:00+00:00       521   PM10  16.76  2021-01-21  15:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     153 2021-01-21 15:50:00+00:00       521    PM1   5.36  2021-01-21  15:50:00
     154 2021-01-21 15:50:00+00:00       521  PM2.5   8.46  2021-01-21  15:50:00
     155 2021-01-21 15:50:00+00:00       521   PM10  18.29  2021-01-21  15:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     159 2021-01-21 16:00:00+00:00       521    PM1   6.02  2021-01-21  16:00:00
     160 2021-01-21 16:00:00+00:00       521  PM2.5   8.48  2021-01-21  16:00:00
     161 2021-01-21 16:00:00+00:00       521   PM10  17.80  2021-01-21  16:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     165 2021-01-21 16:10:00+00:00       521    PM1   6.51  2021-01-21  16:10:00
     166 2021-01-21 16:10:00+00:00       521  PM2.5  10.41  2021-01-21  16:10:00
     167 2021-01-21 16:10:00+00:00       521   PM10  19.79  2021-01-21  16:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     171 2021-01-21 16:20:00+00:00       521    PM1   6.67  2021-01-21  16:20:00
     172 2021-01-21 16:20:00+00:00       521  PM2.5   9.91  2021-01-21  16:20:00
     173 2021-01-21 16:20:00+00:00       521   PM10  19.39  2021-01-21  16:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     177 2021-01-21 16:30:01+00:00       521    PM1   5.75  2021-01-21  16:30:01
     178 2021-01-21 16:30:01+00:00       521  PM2.5   8.50  2021-01-21  16:30:01
     179 2021-01-21 16:30:01+00:00       521   PM10  15.32  2021-01-21  16:30:01,
                         Timestamp  Location   Name  Value        Date      Time
     183 2021-01-21 16:40:00+00:00       521    PM1   5.02  2021-01-21  16:40:00
     184 2021-01-21 16:40:00+00:00       521  PM2.5   6.85  2021-01-21  16:40:00
     185 2021-01-21 16:40:00+00:00       521   PM10  15.05  2021-01-21  16:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     189 2021-01-21 16:49:59+00:00       521    PM1   4.51  2021-01-21  16:49:59
     190 2021-01-21 16:49:59+00:00       521  PM2.5   6.65  2021-01-21  16:49:59
     191 2021-01-21 16:49:59+00:00       521   PM10  13.46  2021-01-21  16:49:59,
                         Timestamp  Location   Name  Value        Date      Time
     195 2021-01-21 17:00:01+00:00       521    PM1   4.33  2021-01-21  17:00:01
     196 2021-01-21 17:00:01+00:00       521  PM2.5   5.84  2021-01-21  17:00:01
     197 2021-01-21 17:00:01+00:00       521   PM10  11.47  2021-01-21  17:00:01,
                         Timestamp  Location   Name  Value        Date      Time
     201 2021-01-21 17:10:00+00:00       521    PM1   4.30  2021-01-21  17:10:00
     202 2021-01-21 17:10:00+00:00       521  PM2.5   5.84  2021-01-21  17:10:00
     203 2021-01-21 17:10:00+00:00       521   PM10  12.42  2021-01-21  17:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     207 2021-01-21 17:20:00+00:00       521    PM1   3.86  2021-01-21  17:20:00
     208 2021-01-21 17:20:00+00:00       521  PM2.5   5.56  2021-01-21  17:20:00
     209 2021-01-21 17:20:00+00:00       521   PM10  10.74  2021-01-21  17:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     213 2021-01-21 17:30:00+00:00       521    PM1   3.98  2021-01-21  17:30:00
     214 2021-01-21 17:30:00+00:00       521  PM2.5   5.64  2021-01-21  17:30:00
     215 2021-01-21 17:30:00+00:00       521   PM10  12.45  2021-01-21  17:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     219 2021-01-21 17:40:00+00:00       521    PM1   4.81  2021-01-21  17:40:00
     220 2021-01-21 17:40:00+00:00       521  PM2.5   6.71  2021-01-21  17:40:00
     221 2021-01-21 17:40:00+00:00       521   PM10  12.58  2021-01-21  17:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     225 2021-01-21 17:50:01+00:00       521    PM1   4.34  2021-01-21  17:50:01
     226 2021-01-21 17:50:01+00:00       521  PM2.5   6.47  2021-01-21  17:50:01
     227 2021-01-21 17:50:01+00:00       521   PM10  13.32  2021-01-21  17:50:01,
                         Timestamp  Location   Name  Value        Date      Time
     231 2021-01-21 18:00:00+00:00       521    PM1   4.13  2021-01-21  18:00:00
     232 2021-01-21 18:00:00+00:00       521  PM2.5   5.72  2021-01-21  18:00:00
     233 2021-01-21 18:00:00+00:00       521   PM10  12.32  2021-01-21  18:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     237 2021-01-21 18:10:00+00:00       521    PM1   4.97  2021-01-21  18:10:00
     238 2021-01-21 18:10:00+00:00       521  PM2.5   6.80  2021-01-21  18:10:00
     239 2021-01-21 18:10:00+00:00       521   PM10  11.20  2021-01-21  18:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     243 2021-01-21 18:20:00+00:00       521    PM1   5.07  2021-01-21  18:20:00
     244 2021-01-21 18:20:00+00:00       521  PM2.5   7.25  2021-01-21  18:20:00
     245 2021-01-21 18:20:00+00:00       521   PM10  14.22  2021-01-21  18:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     249 2021-01-21 18:30:00+00:00       521    PM1   5.66  2021-01-21  18:30:00
     250 2021-01-21 18:30:00+00:00       521  PM2.5   8.32  2021-01-21  18:30:00
     251 2021-01-21 18:30:00+00:00       521   PM10  15.13  2021-01-21  18:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     255 2021-01-21 18:40:00+00:00       521    PM1   5.25  2021-01-21  18:40:00
     256 2021-01-21 18:40:00+00:00       521  PM2.5   7.90  2021-01-21  18:40:00
     257 2021-01-21 18:40:00+00:00       521   PM10  14.37  2021-01-21  18:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     261 2021-01-21 18:50:00+00:00       521    PM1   6.19  2021-01-21  18:50:00
     262 2021-01-21 18:50:00+00:00       521  PM2.5   8.26  2021-01-21  18:50:00
     263 2021-01-21 18:50:00+00:00       521   PM10  16.16  2021-01-21  18:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     267 2021-01-21 19:00:00+00:00       521    PM1   4.99  2021-01-21  19:00:00
     268 2021-01-21 19:00:00+00:00       521  PM2.5   7.41  2021-01-21  19:00:00
     269 2021-01-21 19:00:00+00:00       521   PM10  15.83  2021-01-21  19:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     273 2021-01-21 19:10:00+00:00       521    PM1   6.29  2021-01-21  19:10:00
     274 2021-01-21 19:10:00+00:00       521  PM2.5   8.81  2021-01-21  19:10:00
     275 2021-01-21 19:10:00+00:00       521   PM10  13.02  2021-01-21  19:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     279 2021-01-21 19:20:00+00:00       521    PM1   6.06  2021-01-21  19:20:00
     280 2021-01-21 19:20:00+00:00       521  PM2.5   9.01  2021-01-21  19:20:00
     281 2021-01-21 19:20:00+00:00       521   PM10  16.11  2021-01-21  19:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     285 2021-01-21 19:30:00+00:00       521    PM1   5.41  2021-01-21  19:30:00
     286 2021-01-21 19:30:00+00:00       521  PM2.5   7.92  2021-01-21  19:30:00
     287 2021-01-21 19:30:00+00:00       521   PM10  16.68  2021-01-21  19:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     291 2021-01-21 19:40:00+00:00       521    PM1   5.24  2021-01-21  19:40:00
     292 2021-01-21 19:40:00+00:00       521  PM2.5   7.65  2021-01-21  19:40:00
     293 2021-01-21 19:40:00+00:00       521   PM10  13.14  2021-01-21  19:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     297 2021-01-21 19:50:00+00:00       521    PM1   5.02  2021-01-21  19:50:00
     298 2021-01-21 19:50:00+00:00       521  PM2.5   6.95  2021-01-21  19:50:00
     299 2021-01-21 19:50:00+00:00       521   PM10  11.89  2021-01-21  19:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     303 2021-01-21 20:00:00+00:00       521    PM1   4.31  2021-01-21  20:00:00
     304 2021-01-21 20:00:00+00:00       521  PM2.5   5.04  2021-01-21  20:00:00
     305 2021-01-21 20:00:00+00:00       521   PM10   9.73  2021-01-21  20:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     308 2021-01-21 20:10:00+00:00       521    PM1   4.10  2021-01-21  20:10:00
     309 2021-01-21 20:10:00+00:00       521  PM2.5   4.97  2021-01-21  20:10:00
     310 2021-01-21 20:10:00+00:00       521   PM10   9.89  2021-01-21  20:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     314 2021-01-21 20:20:00+00:00       521    PM1   4.16  2021-01-21  20:20:00
     315 2021-01-21 20:20:00+00:00       521  PM2.5   6.16  2021-01-21  20:20:00
     316 2021-01-21 20:20:00+00:00       521   PM10  10.35  2021-01-21  20:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     320 2021-01-21 20:30:01+00:00       521    PM1   3.95  2021-01-21  20:30:01
     321 2021-01-21 20:30:01+00:00       521  PM2.5   5.59  2021-01-21  20:30:01
     322 2021-01-21 20:30:01+00:00       521   PM10   9.12  2021-01-21  20:30:01,
                         Timestamp  Location   Name  Value        Date      Time
     326 2021-01-21 20:40:00+00:00       521    PM1   3.72  2021-01-21  20:40:00
     327 2021-01-21 20:40:00+00:00       521  PM2.5   4.96  2021-01-21  20:40:00
     328 2021-01-21 20:40:00+00:00       521   PM10   8.71  2021-01-21  20:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     332 2021-01-21 20:50:00+00:00       521    PM1   3.50  2021-01-21  20:50:00
     333 2021-01-21 20:50:00+00:00       521  PM2.5   4.56  2021-01-21  20:50:00
     334 2021-01-21 20:50:00+00:00       521   PM10   6.33  2021-01-21  20:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     338 2021-01-21 21:00:00+00:00       521    PM1   3.27  2021-01-21  21:00:00
     339 2021-01-21 21:00:00+00:00       521  PM2.5   4.09  2021-01-21  21:00:00
     340 2021-01-21 21:00:00+00:00       521   PM10   7.79  2021-01-21  21:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     344 2021-01-21 21:10:00+00:00       521    PM1   3.25  2021-01-21  21:10:00
     345 2021-01-21 21:10:00+00:00       521  PM2.5   4.38  2021-01-21  21:10:00
     346 2021-01-21 21:10:00+00:00       521   PM10   8.19  2021-01-21  21:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     350 2021-01-21 21:20:00+00:00       521    PM1   2.97  2021-01-21  21:20:00
     351 2021-01-21 21:20:00+00:00       521  PM2.5   3.78  2021-01-21  21:20:00
     352 2021-01-21 21:20:00+00:00       521   PM10   7.31  2021-01-21  21:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     356 2021-01-21 21:30:01+00:00       521    PM1   2.82  2021-01-21  21:30:01
     357 2021-01-21 21:30:01+00:00       521  PM2.5   3.32  2021-01-21  21:30:01
     358 2021-01-21 21:30:01+00:00       521   PM10   6.92  2021-01-21  21:30:01,
                         Timestamp  Location   Name  Value        Date      Time
     362 2021-01-21 21:39:59+00:00       521    PM1   2.72  2021-01-21  21:39:59
     363 2021-01-21 21:39:59+00:00       521  PM2.5   3.38  2021-01-21  21:39:59
     364 2021-01-21 21:39:59+00:00       521   PM10   6.15  2021-01-21  21:39:59,
                         Timestamp  Location   Name  Value        Date      Time
     368 2021-01-21 21:50:01+00:00       521    PM1   2.85  2021-01-21  21:50:01
     369 2021-01-21 21:50:01+00:00       521  PM2.5   3.39  2021-01-21  21:50:01
     370 2021-01-21 21:50:01+00:00       521   PM10   5.71  2021-01-21  21:50:01,
                         Timestamp  Location   Name  Value        Date      Time
     374 2021-01-21 22:00:00+00:00       521    PM1   2.39  2021-01-21  22:00:00
     375 2021-01-21 22:00:00+00:00       521  PM2.5   2.60  2021-01-21  22:00:00
     376 2021-01-21 22:00:00+00:00       521   PM10   5.09  2021-01-21  22:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     380 2021-01-21 22:10:00+00:00       521    PM1   2.01  2021-01-21  22:10:00
     381 2021-01-21 22:10:00+00:00       521  PM2.5   2.55  2021-01-21  22:10:00
     382 2021-01-21 22:10:00+00:00       521   PM10   4.48  2021-01-21  22:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     386 2021-01-21 22:20:00+00:00       521    PM1   2.31  2021-01-21  22:20:00
     387 2021-01-21 22:20:00+00:00       521  PM2.5   2.53  2021-01-21  22:20:00
     388 2021-01-21 22:20:00+00:00       521   PM10   5.02  2021-01-21  22:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     392 2021-01-21 22:30:00+00:00       521    PM1   1.85  2021-01-21  22:30:00
     393 2021-01-21 22:30:00+00:00       521  PM2.5   2.64  2021-01-21  22:30:00
     394 2021-01-21 22:30:00+00:00       521   PM10   5.73  2021-01-21  22:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     398 2021-01-21 22:40:00+00:00       521    PM1   2.33  2021-01-21  22:40:00
     399 2021-01-21 22:40:00+00:00       521  PM2.5   2.95  2021-01-21  22:40:00
     400 2021-01-21 22:40:00+00:00       521   PM10   4.84  2021-01-21  22:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     404 2021-01-21 22:50:00+00:00       521    PM1   2.09  2021-01-21  22:50:00
     405 2021-01-21 22:50:00+00:00       521  PM2.5   2.14  2021-01-21  22:50:00
     406 2021-01-21 22:50:00+00:00       521   PM10   4.06  2021-01-21  22:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     410 2021-01-21 23:00:00+00:00       521    PM1   2.61  2021-01-21  23:00:00
     411 2021-01-21 23:00:00+00:00       521  PM2.5   3.48  2021-01-21  23:00:00
     412 2021-01-21 23:00:00+00:00       521   PM10   5.51  2021-01-21  23:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     416 2021-01-21 23:10:00+00:00       521    PM1   3.08  2021-01-21  23:10:00
     417 2021-01-21 23:10:00+00:00       521  PM2.5   3.38  2021-01-21  23:10:00
     418 2021-01-21 23:10:00+00:00       521   PM10   6.15  2021-01-21  23:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     422 2021-01-21 23:20:00+00:00       521    PM1   2.92  2021-01-21  23:20:00
     423 2021-01-21 23:20:00+00:00       521  PM2.5   3.41  2021-01-21  23:20:00
     424 2021-01-21 23:20:00+00:00       521   PM10   5.56  2021-01-21  23:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     428 2021-01-21 23:30:00+00:00       521    PM1   3.56  2021-01-21  23:30:00
     429 2021-01-21 23:30:00+00:00       521  PM2.5   3.84  2021-01-21  23:30:00
     430 2021-01-21 23:30:00+00:00       521   PM10   6.36  2021-01-21  23:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     434 2021-01-21 23:40:00+00:00       521    PM1   4.39  2021-01-21  23:40:00
     435 2021-01-21 23:40:00+00:00       521  PM2.5   5.22  2021-01-21  23:40:00
     436 2021-01-21 23:40:00+00:00       521   PM10   7.68  2021-01-21  23:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     440 2021-01-21 23:50:00+00:00       521    PM1   3.89  2021-01-21  23:50:00
     441 2021-01-21 23:50:00+00:00       521  PM2.5   4.92  2021-01-21  23:50:00
     442 2021-01-21 23:50:00+00:00       521   PM10   8.40  2021-01-21  23:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     446 2021-01-22 00:00:00+00:00       521    PM1   2.77  2021-01-22  00:00:00
     447 2021-01-22 00:00:00+00:00       521  PM2.5   3.42  2021-01-22  00:00:00
     448 2021-01-22 00:00:00+00:00       521   PM10   6.42  2021-01-22  00:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     452 2021-01-22 00:10:01+00:00       521    PM1   2.15  2021-01-22  00:10:01
     453 2021-01-22 00:10:01+00:00       521  PM2.5   2.67  2021-01-22  00:10:01
     454 2021-01-22 00:10:01+00:00       521   PM10   6.53  2021-01-22  00:10:01,
                         Timestamp  Location   Name  Value        Date      Time
     458 2021-01-22 00:20:00+00:00       521    PM1   2.03  2021-01-22  00:20:00
     459 2021-01-22 00:20:00+00:00       521  PM2.5   2.93  2021-01-22  00:20:00
     460 2021-01-22 00:20:00+00:00       521   PM10   7.15  2021-01-22  00:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     464 2021-01-22 00:30:00+00:00       521    PM1   1.95  2021-01-22  00:30:00
     465 2021-01-22 00:30:00+00:00       521  PM2.5   2.52  2021-01-22  00:30:00
     466 2021-01-22 00:30:00+00:00       521   PM10   6.84  2021-01-22  00:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     470 2021-01-22 00:40:00+00:00       521    PM1   2.06  2021-01-22  00:40:00
     471 2021-01-22 00:40:00+00:00       521  PM2.5   2.60  2021-01-22  00:40:00
     472 2021-01-22 00:40:00+00:00       521   PM10   6.32  2021-01-22  00:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     476 2021-01-22 00:50:00+00:00       521    PM1   2.47  2021-01-22  00:50:00
     477 2021-01-22 00:50:00+00:00       521  PM2.5   2.90  2021-01-22  00:50:00
     478 2021-01-22 00:50:00+00:00       521   PM10   6.81  2021-01-22  00:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     482 2021-01-22 01:00:00+00:00       521    PM1   2.29  2021-01-22  01:00:00
     483 2021-01-22 01:00:00+00:00       521  PM2.5   3.13  2021-01-22  01:00:00
     484 2021-01-22 01:00:00+00:00       521   PM10   7.21  2021-01-22  01:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     488 2021-01-22 01:10:00+00:00       521    PM1   2.66  2021-01-22  01:10:00
     489 2021-01-22 01:10:00+00:00       521  PM2.5   3.53  2021-01-22  01:10:00
     490 2021-01-22 01:10:00+00:00       521   PM10   9.42  2021-01-22  01:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     494 2021-01-22 01:20:00+00:00       521    PM1   2.55  2021-01-22  01:20:00
     495 2021-01-22 01:20:00+00:00       521  PM2.5   3.03  2021-01-22  01:20:00
     496 2021-01-22 01:20:00+00:00       521   PM10   6.65  2021-01-22  01:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     500 2021-01-22 01:30:00+00:00       521    PM1   2.80  2021-01-22  01:30:00
     501 2021-01-22 01:30:00+00:00       521  PM2.5   3.76  2021-01-22  01:30:00
     502 2021-01-22 01:30:00+00:00       521   PM10   6.90  2021-01-22  01:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     506 2021-01-22 01:40:00+00:00       521    PM1   2.58  2021-01-22  01:40:00
     507 2021-01-22 01:40:00+00:00       521  PM2.5   3.69  2021-01-22  01:40:00
     508 2021-01-22 01:40:00+00:00       521   PM10   8.33  2021-01-22  01:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     512 2021-01-22 01:50:00+00:00       521    PM1   3.34  2021-01-22  01:50:00
     513 2021-01-22 01:50:00+00:00       521  PM2.5   4.47  2021-01-22  01:50:00
     514 2021-01-22 01:50:00+00:00       521   PM10   8.90  2021-01-22  01:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     518 2021-01-22 02:00:01+00:00       521    PM1   3.52  2021-01-22  02:00:01
     519 2021-01-22 02:00:01+00:00       521  PM2.5   4.84  2021-01-22  02:00:01
     520 2021-01-22 02:00:01+00:00       521   PM10  11.17  2021-01-22  02:00:01,
                         Timestamp  Location   Name  Value        Date      Time
     524 2021-01-22 02:09:59+00:00       521    PM1   4.65  2021-01-22  02:09:59
     525 2021-01-22 02:09:59+00:00       521  PM2.5   5.90  2021-01-22  02:09:59
     526 2021-01-22 02:09:59+00:00       521   PM10  12.01  2021-01-22  02:09:59,
                         Timestamp  Location   Name  Value        Date      Time
     530 2021-01-22 02:20:01+00:00       521    PM1   4.10  2021-01-22  02:20:01
     531 2021-01-22 02:20:01+00:00       521  PM2.5   5.43  2021-01-22  02:20:01
     532 2021-01-22 02:20:01+00:00       521   PM10  14.80  2021-01-22  02:20:01,
                         Timestamp  Location   Name  Value        Date      Time
     536 2021-01-22 02:30:00+00:00       521    PM1   5.36  2021-01-22  02:30:00
     537 2021-01-22 02:30:00+00:00       521  PM2.5   7.86  2021-01-22  02:30:00
     538 2021-01-22 02:30:00+00:00       521   PM10  14.33  2021-01-22  02:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     542 2021-01-22 02:40:00+00:00       521    PM1   4.66  2021-01-22  02:40:00
     543 2021-01-22 02:40:00+00:00       521  PM2.5   7.91  2021-01-22  02:40:00
     544 2021-01-22 02:40:00+00:00       521   PM10  14.82  2021-01-22  02:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     548 2021-01-22 02:50:00+00:00       521    PM1   5.21  2021-01-22  02:50:00
     549 2021-01-22 02:50:00+00:00       521  PM2.5   7.67  2021-01-22  02:50:00
     550 2021-01-22 02:50:00+00:00       521   PM10  18.62  2021-01-22  02:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     554 2021-01-22 03:00:00+00:00       521    PM1   5.34  2021-01-22  03:00:00
     555 2021-01-22 03:00:00+00:00       521  PM2.5   7.77  2021-01-22  03:00:00
     556 2021-01-22 03:00:00+00:00       521   PM10  15.03  2021-01-22  03:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     560 2021-01-22 03:10:00+00:00       521    PM1   5.56  2021-01-22  03:10:00
     561 2021-01-22 03:10:00+00:00       521  PM2.5   7.50  2021-01-22  03:10:00
     562 2021-01-22 03:10:00+00:00       521   PM10  14.92  2021-01-22  03:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     566 2021-01-22 03:20:01+00:00       521    PM1   5.71  2021-01-22  03:20:01
     567 2021-01-22 03:20:01+00:00       521  PM2.5   8.72  2021-01-22  03:20:01
     568 2021-01-22 03:20:01+00:00       521   PM10  19.20  2021-01-22  03:20:01,
                         Timestamp  Location   Name  Value        Date      Time
     572 2021-01-22 03:30:00+00:00       521    PM1   6.06  2021-01-22  03:30:00
     573 2021-01-22 03:30:00+00:00       521  PM2.5   8.79  2021-01-22  03:30:00
     574 2021-01-22 03:30:00+00:00       521   PM10  19.67  2021-01-22  03:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     581 2021-01-22 03:50:00+00:00       521    PM1   5.04  2021-01-22  03:50:00
     582 2021-01-22 03:50:00+00:00       521  PM2.5   8.16  2021-01-22  03:50:00
     583 2021-01-22 03:50:00+00:00       521   PM10  16.90  2021-01-22  03:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     587 2021-01-22 04:00:00+00:00       521    PM1   5.95  2021-01-22  04:00:00
     588 2021-01-22 04:00:00+00:00       521  PM2.5   8.30  2021-01-22  04:00:00
     589 2021-01-22 04:00:00+00:00       521   PM10  17.58  2021-01-22  04:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     593 2021-01-22 04:10:00+00:00       521    PM1   5.47  2021-01-22  04:10:00
     594 2021-01-22 04:10:00+00:00       521  PM2.5   7.96  2021-01-22  04:10:00
     595 2021-01-22 04:10:00+00:00       521   PM10  14.58  2021-01-22  04:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     599 2021-01-22 04:20:00+00:00       521    PM1   5.89  2021-01-22  04:20:00
     600 2021-01-22 04:20:00+00:00       521  PM2.5   9.30  2021-01-22  04:20:00
     601 2021-01-22 04:20:00+00:00       521   PM10  20.92  2021-01-22  04:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     605 2021-01-22 04:30:00+00:00       521    PM1   6.24  2021-01-22  04:30:00
     606 2021-01-22 04:30:00+00:00       521  PM2.5   9.06  2021-01-22  04:30:00
     607 2021-01-22 04:30:00+00:00       521   PM10  19.27  2021-01-22  04:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     611 2021-01-22 04:40:00+00:00       521    PM1   6.77  2021-01-22  04:40:00
     612 2021-01-22 04:40:00+00:00       521  PM2.5   9.88  2021-01-22  04:40:00
     613 2021-01-22 04:40:00+00:00       521   PM10  23.03  2021-01-22  04:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     617 2021-01-22 04:50:00+00:00       521    PM1   6.76  2021-01-22  04:50:00
     618 2021-01-22 04:50:00+00:00       521  PM2.5  10.01  2021-01-22  04:50:00
     619 2021-01-22 04:50:00+00:00       521   PM10  24.61  2021-01-22  04:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     623 2021-01-22 05:00:00+00:00       521    PM1   7.07  2021-01-22  05:00:00
     624 2021-01-22 05:00:00+00:00       521  PM2.5  10.30  2021-01-22  05:00:00
     625 2021-01-22 05:00:00+00:00       521   PM10  28.24  2021-01-22  05:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     629 2021-01-22 05:10:00+00:00       521    PM1   7.32  2021-01-22  05:10:00
     630 2021-01-22 05:10:00+00:00       521  PM2.5  10.17  2021-01-22  05:10:00
     631 2021-01-22 05:10:00+00:00       521   PM10  21.66  2021-01-22  05:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     635 2021-01-22 05:20:00+00:00       521    PM1   6.92  2021-01-22  05:20:00
     636 2021-01-22 05:20:00+00:00       521  PM2.5  10.97  2021-01-22  05:20:00
     637 2021-01-22 05:20:00+00:00       521   PM10  24.57  2021-01-22  05:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     641 2021-01-22 05:30:00+00:00       521    PM1   6.90  2021-01-22  05:30:00
     642 2021-01-22 05:30:00+00:00       521  PM2.5  10.90  2021-01-22  05:30:00
     643 2021-01-22 05:30:00+00:00       521   PM10  21.67  2021-01-22  05:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     647 2021-01-22 05:40:01+00:00       521    PM1   7.29  2021-01-22  05:40:01
     648 2021-01-22 05:40:01+00:00       521  PM2.5  12.05  2021-01-22  05:40:01
     649 2021-01-22 05:40:01+00:00       521   PM10  23.26  2021-01-22  05:40:01,
                         Timestamp  Location   Name  Value        Date      Time
     653 2021-01-22 05:50:00+00:00       521    PM1   7.69  2021-01-22  05:50:00
     654 2021-01-22 05:50:00+00:00       521  PM2.5  11.47  2021-01-22  05:50:00
     655 2021-01-22 05:50:00+00:00       521   PM10  23.92  2021-01-22  05:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     659 2021-01-22 06:00:00+00:00       521    PM1   7.79  2021-01-22  06:00:00
     660 2021-01-22 06:00:00+00:00       521  PM2.5  11.71  2021-01-22  06:00:00
     661 2021-01-22 06:00:00+00:00       521   PM10  25.43  2021-01-22  06:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     665 2021-01-22 06:10:00+00:00       521    PM1   7.22  2021-01-22  06:10:00
     666 2021-01-22 06:10:00+00:00       521  PM2.5  11.24  2021-01-22  06:10:00
     667 2021-01-22 06:10:00+00:00       521   PM10  23.33  2021-01-22  06:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     671 2021-01-22 06:20:00+00:00       521    PM1   7.89  2021-01-22  06:20:00
     672 2021-01-22 06:20:00+00:00       521  PM2.5  11.93  2021-01-22  06:20:00
     673 2021-01-22 06:20:00+00:00       521   PM10  22.12  2021-01-22  06:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     677 2021-01-22 06:30:00+00:00       521    PM1   6.80  2021-01-22  06:30:00
     678 2021-01-22 06:30:00+00:00       521  PM2.5   9.90  2021-01-22  06:30:00
     679 2021-01-22 06:30:00+00:00       521   PM10  23.05  2021-01-22  06:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     683 2021-01-22 06:40:00+00:00       521    PM1   7.47  2021-01-22  06:40:00
     684 2021-01-22 06:40:00+00:00       521  PM2.5  11.29  2021-01-22  06:40:00
     685 2021-01-22 06:40:00+00:00       521   PM10  19.31  2021-01-22  06:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     689 2021-01-22 06:50:01+00:00       521    PM1   8.37  2021-01-22  06:50:01
     690 2021-01-22 06:50:01+00:00       521  PM2.5  11.17  2021-01-22  06:50:01
     691 2021-01-22 06:50:01+00:00       521   PM10  19.18  2021-01-22  06:50:01,
                         Timestamp  Location   Name  Value        Date      Time
     695 2021-01-22 06:59:59+00:00       521    PM1   6.74  2021-01-22  06:59:59
     696 2021-01-22 06:59:59+00:00       521  PM2.5   9.87  2021-01-22  06:59:59
     697 2021-01-22 06:59:59+00:00       521   PM10  18.74  2021-01-22  06:59:59,
                         Timestamp  Location   Name  Value        Date      Time
     701 2021-01-22 07:10:01+00:00       521    PM1   6.99  2021-01-22  07:10:01
     702 2021-01-22 07:10:01+00:00       521  PM2.5  11.17  2021-01-22  07:10:01
     703 2021-01-22 07:10:01+00:00       521   PM10  21.23  2021-01-22  07:10:01,
                         Timestamp  Location   Name  Value        Date      Time
     707 2021-01-22 07:20:00+00:00       521    PM1   6.73  2021-01-22  07:20:00
     708 2021-01-22 07:20:00+00:00       521  PM2.5  10.59  2021-01-22  07:20:00
     709 2021-01-22 07:20:00+00:00       521   PM10  21.01  2021-01-22  07:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     713 2021-01-22 07:30:00+00:00       521    PM1   6.61  2021-01-22  07:30:00
     714 2021-01-22 07:30:00+00:00       521  PM2.5   9.89  2021-01-22  07:30:00
     715 2021-01-22 07:30:00+00:00       521   PM10  18.93  2021-01-22  07:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     719 2021-01-22 07:40:00+00:00       521    PM1   7.39  2021-01-22  07:40:00
     720 2021-01-22 07:40:00+00:00       521  PM2.5  11.11  2021-01-22  07:40:00
     721 2021-01-22 07:40:00+00:00       521   PM10  26.30  2021-01-22  07:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     725 2021-01-22 07:50:00+00:00       521    PM1   6.84  2021-01-22  07:50:00
     726 2021-01-22 07:50:00+00:00       521  PM2.5  11.00  2021-01-22  07:50:00
     727 2021-01-22 07:50:00+00:00       521   PM10  23.02  2021-01-22  07:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     731 2021-01-22 08:00:00+00:00       521    PM1   7.61  2021-01-22  08:00:00
     732 2021-01-22 08:00:00+00:00       521  PM2.5  12.03  2021-01-22  08:00:00
     733 2021-01-22 08:00:00+00:00       521   PM10  24.18  2021-01-22  08:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     737 2021-01-22 08:10:00+00:00       521    PM1   7.19  2021-01-22  08:10:00
     738 2021-01-22 08:10:00+00:00       521  PM2.5  11.21  2021-01-22  08:10:00
     739 2021-01-22 08:10:00+00:00       521   PM10  21.10  2021-01-22  08:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     743 2021-01-22 08:20:00+00:00       521    PM1   7.63  2021-01-22  08:20:00
     744 2021-01-22 08:20:00+00:00       521  PM2.5  11.09  2021-01-22  08:20:00
     745 2021-01-22 08:20:00+00:00       521   PM10  21.22  2021-01-22  08:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     749 2021-01-22 08:30:00+00:00       521    PM1   7.14  2021-01-22  08:30:00
     750 2021-01-22 08:30:00+00:00       521  PM2.5  10.07  2021-01-22  08:30:00
     751 2021-01-22 08:30:00+00:00       521   PM10  20.14  2021-01-22  08:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     755 2021-01-22 08:40:00+00:00       521    PM1   6.71  2021-01-22  08:40:00
     756 2021-01-22 08:40:00+00:00       521  PM2.5   9.81  2021-01-22  08:40:00
     757 2021-01-22 08:40:00+00:00       521   PM10  22.72  2021-01-22  08:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     761 2021-01-22 08:50:00+00:00       521    PM1   6.83  2021-01-22  08:50:00
     762 2021-01-22 08:50:00+00:00       521  PM2.5  11.04  2021-01-22  08:50:00
     763 2021-01-22 08:50:00+00:00       521   PM10  22.06  2021-01-22  08:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     767 2021-01-22 09:00:00+00:00       521    PM1   7.51  2021-01-22  09:00:00
     768 2021-01-22 09:00:00+00:00       521  PM2.5  11.77  2021-01-22  09:00:00
     769 2021-01-22 09:00:00+00:00       521   PM10  21.82  2021-01-22  09:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     773 2021-01-22 09:10:00+00:00       521    PM1   7.17  2021-01-22  09:10:00
     774 2021-01-22 09:10:00+00:00       521  PM2.5  10.47  2021-01-22  09:10:00
     775 2021-01-22 09:10:00+00:00       521   PM10  20.48  2021-01-22  09:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     779 2021-01-22 09:20:00+00:00       521    PM1   7.46  2021-01-22  09:20:00
     780 2021-01-22 09:20:00+00:00       521  PM2.5  10.17  2021-01-22  09:20:00
     781 2021-01-22 09:20:00+00:00       521   PM10  22.18  2021-01-22  09:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     785 2021-01-22 09:30:01+00:00       521    PM1   7.56  2021-01-22  09:30:01
     786 2021-01-22 09:30:01+00:00       521  PM2.5  10.78  2021-01-22  09:30:01
     787 2021-01-22 09:30:01+00:00       521   PM10  24.23  2021-01-22  09:30:01,
                         Timestamp  Location   Name  Value        Date      Time
     791 2021-01-22 09:40:00+00:00       521    PM1   6.84  2021-01-22  09:40:00
     792 2021-01-22 09:40:00+00:00       521  PM2.5   9.76  2021-01-22  09:40:00
     793 2021-01-22 09:40:00+00:00       521   PM10  24.10  2021-01-22  09:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     797 2021-01-22 09:50:00+00:00       521    PM1   7.03  2021-01-22  09:50:00
     798 2021-01-22 09:50:00+00:00       521  PM2.5  11.19  2021-01-22  09:50:00
     799 2021-01-22 09:50:00+00:00       521   PM10  19.85  2021-01-22  09:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     803 2021-01-22 10:00:00+00:00       521    PM1   7.58  2021-01-22  10:00:00
     804 2021-01-22 10:00:00+00:00       521  PM2.5  11.28  2021-01-22  10:00:00
     805 2021-01-22 10:00:00+00:00       521   PM10  26.52  2021-01-22  10:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     809 2021-01-22 10:10:00+00:00       521    PM1   6.31  2021-01-22  10:10:00
     810 2021-01-22 10:10:00+00:00       521  PM2.5  10.39  2021-01-22  10:10:00
     811 2021-01-22 10:10:00+00:00       521   PM10  25.87  2021-01-22  10:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     815 2021-01-22 10:20:00+00:00       521    PM1   8.19  2021-01-22  10:20:00
     816 2021-01-22 10:20:00+00:00       521  PM2.5  12.60  2021-01-22  10:20:00
     817 2021-01-22 10:20:00+00:00       521   PM10  24.78  2021-01-22  10:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     821 2021-01-22 10:30:00+00:00       521    PM1   6.58  2021-01-22  10:30:00
     822 2021-01-22 10:30:00+00:00       521  PM2.5  11.58  2021-01-22  10:30:00
     823 2021-01-22 10:30:00+00:00       521   PM10  22.97  2021-01-22  10:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     827 2021-01-22 10:40:00+00:00       521    PM1   6.24  2021-01-22  10:40:00
     828 2021-01-22 10:40:00+00:00       521  PM2.5   9.29  2021-01-22  10:40:00
     829 2021-01-22 10:40:00+00:00       521   PM10  24.21  2021-01-22  10:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     833 2021-01-22 10:50:01+00:00       521    PM1   6.23  2021-01-22  10:50:01
     834 2021-01-22 10:50:01+00:00       521  PM2.5   9.85  2021-01-22  10:50:01
     835 2021-01-22 10:50:01+00:00       521   PM10  22.75  2021-01-22  10:50:01,
                         Timestamp  Location   Name  Value        Date      Time
     839 2021-01-22 11:00:00+00:00       521    PM1   6.93  2021-01-22  11:00:00
     840 2021-01-22 11:00:00+00:00       521  PM2.5  11.29  2021-01-22  11:00:00
     841 2021-01-22 11:00:00+00:00       521   PM10  21.04  2021-01-22  11:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     845 2021-01-22 11:10:00+00:00       521    PM1   5.91  2021-01-22  11:10:00
     846 2021-01-22 11:10:00+00:00       521  PM2.5   8.92  2021-01-22  11:10:00
     847 2021-01-22 11:10:00+00:00       521   PM10  19.64  2021-01-22  11:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     851 2021-01-22 11:20:00+00:00       521    PM1   7.20  2021-01-22  11:20:00
     852 2021-01-22 11:20:00+00:00       521  PM2.5  11.00  2021-01-22  11:20:00
     853 2021-01-22 11:20:00+00:00       521   PM10  24.78  2021-01-22  11:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     857 2021-01-22 11:30:00+00:00       521    PM1   7.53  2021-01-22  11:30:00
     858 2021-01-22 11:30:00+00:00       521  PM2.5  11.57  2021-01-22  11:30:00
     859 2021-01-22 11:30:00+00:00       521   PM10  27.26  2021-01-22  11:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     863 2021-01-22 11:40:00+00:00       521    PM1   7.40  2021-01-22  11:40:00
     864 2021-01-22 11:40:00+00:00       521  PM2.5  11.18  2021-01-22  11:40:00
     865 2021-01-22 11:40:00+00:00       521   PM10  23.92  2021-01-22  11:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     869 2021-01-22 11:50:00+00:00       521    PM1   6.72  2021-01-22  11:50:00
     870 2021-01-22 11:50:00+00:00       521  PM2.5  10.80  2021-01-22  11:50:00
     871 2021-01-22 11:50:00+00:00       521   PM10  23.43  2021-01-22  11:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     875 2021-01-22 12:00:00+00:00       521    PM1   6.56  2021-01-22  12:00:00
     876 2021-01-22 12:00:00+00:00       521  PM2.5  10.80  2021-01-22  12:00:00
     877 2021-01-22 12:00:00+00:00       521   PM10  21.59  2021-01-22  12:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     881 2021-01-22 12:10:00+00:00       521    PM1   6.18  2021-01-22  12:10:00
     882 2021-01-22 12:10:00+00:00       521  PM2.5   9.32  2021-01-22  12:10:00
     883 2021-01-22 12:10:00+00:00       521   PM10  21.20  2021-01-22  12:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     887 2021-01-22 12:20:00+00:00       521    PM1   6.85  2021-01-22  12:20:00
     888 2021-01-22 12:20:00+00:00       521  PM2.5   9.90  2021-01-22  12:20:00
     889 2021-01-22 12:20:00+00:00       521   PM10  21.86  2021-01-22  12:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     893 2021-01-22 12:30:00+00:00       521    PM1   6.86  2021-01-22  12:30:00
     894 2021-01-22 12:30:00+00:00       521  PM2.5  10.81  2021-01-22  12:30:00
     895 2021-01-22 12:30:00+00:00       521   PM10  26.98  2021-01-22  12:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     899 2021-01-22 12:40:00+00:00       521    PM1   6.24  2021-01-22  12:40:00
     900 2021-01-22 12:40:00+00:00       521  PM2.5   9.83  2021-01-22  12:40:00
     901 2021-01-22 12:40:00+00:00       521   PM10  21.63  2021-01-22  12:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     905 2021-01-22 12:50:01+00:00       521    PM1   6.26  2021-01-22  12:50:01
     906 2021-01-22 12:50:01+00:00       521  PM2.5  10.38  2021-01-22  12:50:01
     907 2021-01-22 12:50:01+00:00       521   PM10  19.58  2021-01-22  12:50:01,
                         Timestamp  Location   Name  Value        Date      Time
     911 2021-01-22 13:00:00+00:00       521    PM1   7.77  2021-01-22  13:00:00
     912 2021-01-22 13:00:00+00:00       521  PM2.5  11.00  2021-01-22  13:00:00
     913 2021-01-22 13:00:00+00:00       521   PM10  26.45  2021-01-22  13:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     917 2021-01-22 13:10:00+00:00       521    PM1   6.13  2021-01-22  13:10:00
     918 2021-01-22 13:10:00+00:00       521  PM2.5   8.98  2021-01-22  13:10:00
     919 2021-01-22 13:10:00+00:00       521   PM10  23.64  2021-01-22  13:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     923 2021-01-22 13:20:00+00:00       521    PM1   7.07  2021-01-22  13:20:00
     924 2021-01-22 13:20:00+00:00       521  PM2.5   9.79  2021-01-22  13:20:00
     925 2021-01-22 13:20:00+00:00       521   PM10  19.32  2021-01-22  13:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     929 2021-01-22 13:30:00+00:00       521    PM1   6.40  2021-01-22  13:30:00
     930 2021-01-22 13:30:00+00:00       521  PM2.5   9.69  2021-01-22  13:30:00
     931 2021-01-22 13:30:00+00:00       521   PM10  23.65  2021-01-22  13:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     935 2021-01-22 13:40:00+00:00       521    PM1   6.94  2021-01-22  13:40:00
     936 2021-01-22 13:40:00+00:00       521  PM2.5  10.36  2021-01-22  13:40:00
     937 2021-01-22 13:40:00+00:00       521   PM10  25.73  2021-01-22  13:40:00,
                         Timestamp  Location   Name  Value        Date      Time
     941 2021-01-22 13:50:00+00:00       521    PM1   7.12  2021-01-22  13:50:00
     942 2021-01-22 13:50:00+00:00       521  PM2.5  10.52  2021-01-22  13:50:00
     943 2021-01-22 13:50:00+00:00       521   PM10  22.69  2021-01-22  13:50:00,
                         Timestamp  Location   Name  Value        Date      Time
     947 2021-01-22 14:00:00+00:00       521    PM1   6.71  2021-01-22  14:00:00
     948 2021-01-22 14:00:00+00:00       521  PM2.5  10.14  2021-01-22  14:00:00
     949 2021-01-22 14:00:00+00:00       521   PM10  19.41  2021-01-22  14:00:00,
                         Timestamp  Location   Name  Value        Date      Time
     953 2021-01-22 14:10:00+00:00       521    PM1   6.50  2021-01-22  14:10:00
     954 2021-01-22 14:10:00+00:00       521  PM2.5  10.29  2021-01-22  14:10:00
     955 2021-01-22 14:10:00+00:00       521   PM10  21.88  2021-01-22  14:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     959 2021-01-22 14:20:00+00:00       521    PM1   6.95  2021-01-22  14:20:00
     960 2021-01-22 14:20:00+00:00       521  PM2.5   9.24  2021-01-22  14:20:00
     961 2021-01-22 14:20:00+00:00       521   PM10  20.64  2021-01-22  14:20:00,
                         Timestamp  Location   Name  Value        Date      Time
     965 2021-01-22 14:30:00+00:00       521    PM1   7.28  2021-01-22  14:30:00
     966 2021-01-22 14:30:00+00:00       521  PM2.5  10.39  2021-01-22  14:30:00
     967 2021-01-22 14:30:00+00:00       521   PM10  20.93  2021-01-22  14:30:00,
                         Timestamp  Location   Name  Value        Date      Time
     971 2021-01-22 14:40:01+00:00       521    PM1   7.33  2021-01-22  14:40:01
     972 2021-01-22 14:40:01+00:00       521  PM2.5  11.07  2021-01-22  14:40:01
     973 2021-01-22 14:40:01+00:00       521   PM10  24.62  2021-01-22  14:40:01,
                         Timestamp  Location   Name  Value        Date      Time
     977 2021-01-22 14:49:59+00:00       521    PM1   6.10  2021-01-22  14:49:59
     978 2021-01-22 14:49:59+00:00       521  PM2.5   9.72  2021-01-22  14:49:59
     979 2021-01-22 14:49:59+00:00       521   PM10  23.51  2021-01-22  14:49:59,
                         Timestamp  Location   Name  Value        Date      Time
     983 2021-01-22 15:00:01+00:00       521    PM1   6.88  2021-01-22  15:00:01
     984 2021-01-22 15:00:01+00:00       521  PM2.5  11.28  2021-01-22  15:00:01
     985 2021-01-22 15:00:01+00:00       521   PM10  22.00  2021-01-22  15:00:01,
                         Timestamp  Location   Name  Value        Date      Time
     989 2021-01-22 15:10:00+00:00       521    PM1   6.30  2021-01-22  15:10:00
     990 2021-01-22 15:10:00+00:00       521  PM2.5   9.60  2021-01-22  15:10:00
     991 2021-01-22 15:10:00+00:00       521   PM10  20.34  2021-01-22  15:10:00,
                         Timestamp  Location   Name  Value        Date      Time
     995 2021-01-22 15:20:00+00:00       521    PM1   6.73  2021-01-22  15:20:00
     996 2021-01-22 15:20:00+00:00       521  PM2.5  11.12  2021-01-22  15:20:00
     997 2021-01-22 15:20:00+00:00       521   PM10  20.35  2021-01-22  15:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1001 2021-01-22 15:30:00+00:00       521    PM1   6.70  2021-01-22  15:30:00
     1002 2021-01-22 15:30:00+00:00       521  PM2.5   9.49  2021-01-22  15:30:00
     1003 2021-01-22 15:30:00+00:00       521   PM10  19.87  2021-01-22  15:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1007 2021-01-22 15:40:00+00:00       521    PM1   7.01  2021-01-22  15:40:00
     1008 2021-01-22 15:40:00+00:00       521  PM2.5  10.43  2021-01-22  15:40:00
     1009 2021-01-22 15:40:00+00:00       521   PM10  24.50  2021-01-22  15:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1013 2021-01-22 15:50:00+00:00       521    PM1   6.53  2021-01-22  15:50:00
     1014 2021-01-22 15:50:00+00:00       521  PM2.5  10.07  2021-01-22  15:50:00
     1015 2021-01-22 15:50:00+00:00       521   PM10  22.06  2021-01-22  15:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1019 2021-01-22 16:00:00+00:00       521    PM1   6.96  2021-01-22  16:00:00
     1020 2021-01-22 16:00:00+00:00       521  PM2.5  10.58  2021-01-22  16:00:00
     1021 2021-01-22 16:00:00+00:00       521   PM10  17.68  2021-01-22  16:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1025 2021-01-22 16:10:00+00:00       521    PM1   6.20  2021-01-22  16:10:00
     1026 2021-01-22 16:10:00+00:00       521  PM2.5   9.45  2021-01-22  16:10:00
     1027 2021-01-22 16:10:00+00:00       521   PM10  23.82  2021-01-22  16:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1031 2021-01-22 16:20:01+00:00       521    PM1   7.81  2021-01-22  16:20:01
     1032 2021-01-22 16:20:01+00:00       521  PM2.5  11.48  2021-01-22  16:20:01
     1033 2021-01-22 16:20:01+00:00       521   PM10  20.74  2021-01-22  16:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     1037 2021-01-22 16:29:59+00:00       521    PM1   6.86  2021-01-22  16:29:59
     1038 2021-01-22 16:29:59+00:00       521  PM2.5   9.35  2021-01-22  16:29:59
     1039 2021-01-22 16:29:59+00:00       521   PM10  20.65  2021-01-22  16:29:59,
                          Timestamp  Location   Name  Value        Date      Time
     1043 2021-01-22 16:40:01+00:00       521    PM1   6.09  2021-01-22  16:40:01
     1044 2021-01-22 16:40:01+00:00       521  PM2.5   8.90  2021-01-22  16:40:01
     1045 2021-01-22 16:40:01+00:00       521   PM10  16.53  2021-01-22  16:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     1049 2021-01-22 16:50:00+00:00       521    PM1   6.01  2021-01-22  16:50:00
     1050 2021-01-22 16:50:00+00:00       521  PM2.5   8.74  2021-01-22  16:50:00
     1051 2021-01-22 16:50:00+00:00       521   PM10  17.26  2021-01-22  16:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1055 2021-01-22 17:00:00+00:00       521    PM1   5.81  2021-01-22  17:00:00
     1056 2021-01-22 17:00:00+00:00       521  PM2.5   9.14  2021-01-22  17:00:00
     1057 2021-01-22 17:00:00+00:00       521   PM10  15.79  2021-01-22  17:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1061 2021-01-22 17:10:00+00:00       521    PM1   5.42  2021-01-22  17:10:00
     1062 2021-01-22 17:10:00+00:00       521  PM2.5   8.10  2021-01-22  17:10:00
     1063 2021-01-22 17:10:00+00:00       521   PM10  18.81  2021-01-22  17:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1067 2021-01-22 17:20:00+00:00       521    PM1   6.49  2021-01-22  17:20:00
     1068 2021-01-22 17:20:00+00:00       521  PM2.5   8.82  2021-01-22  17:20:00
     1069 2021-01-22 17:20:00+00:00       521   PM10  17.99  2021-01-22  17:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1073 2021-01-22 17:30:00+00:00       521    PM1   6.96  2021-01-22  17:30:00
     1074 2021-01-22 17:30:00+00:00       521  PM2.5   9.30  2021-01-22  17:30:00
     1075 2021-01-22 17:30:00+00:00       521   PM10  19.00  2021-01-22  17:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1079 2021-01-22 17:40:00+00:00       521    PM1   7.26  2021-01-22  17:40:00
     1080 2021-01-22 17:40:00+00:00       521  PM2.5  11.03  2021-01-22  17:40:00
     1081 2021-01-22 17:40:00+00:00       521   PM10  17.41  2021-01-22  17:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1085 2021-01-22 17:50:00+00:00       521    PM1   6.36  2021-01-22  17:50:00
     1086 2021-01-22 17:50:00+00:00       521  PM2.5   8.83  2021-01-22  17:50:00
     1087 2021-01-22 17:50:00+00:00       521   PM10  16.36  2021-01-22  17:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1091 2021-01-22 18:00:00+00:00       521    PM1   6.81  2021-01-22  18:00:00
     1092 2021-01-22 18:00:00+00:00       521  PM2.5   9.92  2021-01-22  18:00:00
     1093 2021-01-22 18:00:00+00:00       521   PM10  18.60  2021-01-22  18:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1097 2021-01-22 18:10:00+00:00       521    PM1   6.26  2021-01-22  18:10:00
     1098 2021-01-22 18:10:00+00:00       521  PM2.5   9.41  2021-01-22  18:10:00
     1099 2021-01-22 18:10:00+00:00       521   PM10  16.71  2021-01-22  18:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1103 2021-01-22 18:20:00+00:00       521    PM1   6.55  2021-01-22  18:20:00
     1104 2021-01-22 18:20:00+00:00       521  PM2.5   9.34  2021-01-22  18:20:00
     1105 2021-01-22 18:20:00+00:00       521   PM10  18.89  2021-01-22  18:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1109 2021-01-22 18:30:01+00:00       521    PM1   7.05  2021-01-22  18:30:01
     1110 2021-01-22 18:30:01+00:00       521  PM2.5  10.58  2021-01-22  18:30:01
     1111 2021-01-22 18:30:01+00:00       521   PM10  18.09  2021-01-22  18:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     1115 2021-01-22 18:39:59+00:00       521    PM1   7.01  2021-01-22  18:39:59
     1116 2021-01-22 18:39:59+00:00       521  PM2.5  10.33  2021-01-22  18:39:59
     1117 2021-01-22 18:39:59+00:00       521   PM10  20.18  2021-01-22  18:39:59,
                          Timestamp  Location   Name  Value        Date      Time
     1121 2021-01-22 18:50:01+00:00       521    PM1   7.07  2021-01-22  18:50:01
     1122 2021-01-22 18:50:01+00:00       521  PM2.5   9.68  2021-01-22  18:50:01
     1123 2021-01-22 18:50:01+00:00       521   PM10  19.72  2021-01-22  18:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     1127 2021-01-22 19:00:00+00:00       521    PM1   7.10  2021-01-22  19:00:00
     1128 2021-01-22 19:00:00+00:00       521  PM2.5  10.25  2021-01-22  19:00:00
     1129 2021-01-22 19:00:00+00:00       521   PM10  15.08  2021-01-22  19:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1133 2021-01-22 19:10:00+00:00       521    PM1   7.15  2021-01-22  19:10:00
     1134 2021-01-22 19:10:00+00:00       521  PM2.5  10.16  2021-01-22  19:10:00
     1135 2021-01-22 19:10:00+00:00       521   PM10  18.78  2021-01-22  19:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1139 2021-01-22 19:20:00+00:00       521    PM1   6.80  2021-01-22  19:20:00
     1140 2021-01-22 19:20:00+00:00       521  PM2.5  10.09  2021-01-22  19:20:00
     1141 2021-01-22 19:20:00+00:00       521   PM10  19.39  2021-01-22  19:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1145 2021-01-22 19:30:00+00:00       521    PM1   6.67  2021-01-22  19:30:00
     1146 2021-01-22 19:30:00+00:00       521  PM2.5   9.64  2021-01-22  19:30:00
     1147 2021-01-22 19:30:00+00:00       521   PM10  18.27  2021-01-22  19:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1151 2021-01-22 19:40:00+00:00       521    PM1   8.61  2021-01-22  19:40:00
     1152 2021-01-22 19:40:00+00:00       521  PM2.5  11.55  2021-01-22  19:40:00
     1153 2021-01-22 19:40:00+00:00       521   PM10  18.38  2021-01-22  19:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1157 2021-01-22 19:50:00+00:00       521    PM1   7.96  2021-01-22  19:50:00
     1158 2021-01-22 19:50:00+00:00       521  PM2.5  10.44  2021-01-22  19:50:00
     1159 2021-01-22 19:50:00+00:00       521   PM10  18.68  2021-01-22  19:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1163 2021-01-22 20:00:00+00:00       521    PM1   9.16  2021-01-22  20:00:00
     1164 2021-01-22 20:00:00+00:00       521  PM2.5  12.19  2021-01-22  20:00:00
     1165 2021-01-22 20:00:00+00:00       521   PM10  20.47  2021-01-22  20:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1169 2021-01-22 20:10:00+00:00       521    PM1   7.81  2021-01-22  20:10:00
     1170 2021-01-22 20:10:00+00:00       521  PM2.5  10.62  2021-01-22  20:10:00
     1171 2021-01-22 20:10:00+00:00       521   PM10  16.70  2021-01-22  20:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1175 2021-01-22 20:20:01+00:00       521    PM1   7.82  2021-01-22  20:20:01
     1176 2021-01-22 20:20:01+00:00       521  PM2.5  11.34  2021-01-22  20:20:01
     1177 2021-01-22 20:20:01+00:00       521   PM10  19.24  2021-01-22  20:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     1181 2021-01-22 20:30:00+00:00       521    PM1   7.88  2021-01-22  20:30:00
     1182 2021-01-22 20:30:00+00:00       521  PM2.5  10.93  2021-01-22  20:30:00
     1183 2021-01-22 20:30:00+00:00       521   PM10  24.02  2021-01-22  20:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1187 2021-01-22 20:40:00+00:00       521    PM1   9.24  2021-01-22  20:40:00
     1188 2021-01-22 20:40:00+00:00       521  PM2.5  12.90  2021-01-22  20:40:00
     1189 2021-01-22 20:40:00+00:00       521   PM10  23.26  2021-01-22  20:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1193 2021-01-22 20:50:00+00:00       521    PM1   9.56  2021-01-22  20:50:00
     1194 2021-01-22 20:50:00+00:00       521  PM2.5  13.45  2021-01-22  20:50:00
     1195 2021-01-22 20:50:00+00:00       521   PM10  27.92  2021-01-22  20:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1199 2021-01-22 21:00:00+00:00       521    PM1  10.26  2021-01-22  21:00:00
     1200 2021-01-22 21:00:00+00:00       521  PM2.5  14.41  2021-01-22  21:00:00
     1201 2021-01-22 21:00:00+00:00       521   PM10  30.08  2021-01-22  21:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1205 2021-01-22 21:10:00+00:00       521    PM1  10.07  2021-01-22  21:10:00
     1206 2021-01-22 21:10:00+00:00       521  PM2.5  14.47  2021-01-22  21:10:00
     1207 2021-01-22 21:10:00+00:00       521   PM10  26.36  2021-01-22  21:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1211 2021-01-22 21:20:00+00:00       521    PM1  10.07  2021-01-22  21:20:00
     1212 2021-01-22 21:20:00+00:00       521  PM2.5  14.60  2021-01-22  21:20:00
     1213 2021-01-22 21:20:00+00:00       521   PM10  27.75  2021-01-22  21:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1217 2021-01-22 21:30:00+00:00       521    PM1   9.53  2021-01-22  21:30:00
     1218 2021-01-22 21:30:00+00:00       521  PM2.5  12.51  2021-01-22  21:30:00
     1219 2021-01-22 21:30:00+00:00       521   PM10  23.57  2021-01-22  21:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1223 2021-01-22 21:40:00+00:00       521    PM1   9.21  2021-01-22  21:40:00
     1224 2021-01-22 21:40:00+00:00       521  PM2.5  12.74  2021-01-22  21:40:00
     1225 2021-01-22 21:40:00+00:00       521   PM10  25.12  2021-01-22  21:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1229 2021-01-22 21:50:00+00:00       521    PM1   9.24  2021-01-22  21:50:00
     1230 2021-01-22 21:50:00+00:00       521  PM2.5  12.37  2021-01-22  21:50:00
     1231 2021-01-22 21:50:00+00:00       521   PM10  22.75  2021-01-22  21:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1235 2021-01-22 22:00:00+00:00       521    PM1   8.31  2021-01-22  22:00:00
     1236 2021-01-22 22:00:00+00:00       521  PM2.5  11.09  2021-01-22  22:00:00
     1237 2021-01-22 22:00:00+00:00       521   PM10  24.91  2021-01-22  22:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1241 2021-01-22 22:10:00+00:00       521    PM1   9.36  2021-01-22  22:10:00
     1242 2021-01-22 22:10:00+00:00       521  PM2.5  11.94  2021-01-22  22:10:00
     1243 2021-01-22 22:10:00+00:00       521   PM10  20.76  2021-01-22  22:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1247 2021-01-22 22:20:01+00:00       521    PM1   9.17  2021-01-22  22:20:01
     1248 2021-01-22 22:20:01+00:00       521  PM2.5  11.77  2021-01-22  22:20:01
     1249 2021-01-22 22:20:01+00:00       521   PM10  19.49  2021-01-22  22:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     1253 2021-01-22 22:30:00+00:00       521    PM1   9.13  2021-01-22  22:30:00
     1254 2021-01-22 22:30:00+00:00       521  PM2.5  11.46  2021-01-22  22:30:00
     1255 2021-01-22 22:30:00+00:00       521   PM10  18.13  2021-01-22  22:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1259 2021-01-22 22:40:00+00:00       521    PM1   9.48  2021-01-22  22:40:00
     1260 2021-01-22 22:40:00+00:00       521  PM2.5  11.27  2021-01-22  22:40:00
     1261 2021-01-22 22:40:00+00:00       521   PM10  20.27  2021-01-22  22:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1265 2021-01-22 22:50:00+00:00       521    PM1   9.66  2021-01-22  22:50:00
     1266 2021-01-22 22:50:00+00:00       521  PM2.5  12.23  2021-01-22  22:50:00
     1267 2021-01-22 22:50:00+00:00       521   PM10  19.87  2021-01-22  22:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1271 2021-01-22 23:00:00+00:00       521    PM1   9.13  2021-01-22  23:00:00
     1272 2021-01-22 23:00:00+00:00       521  PM2.5  11.84  2021-01-22  23:00:00
     1273 2021-01-22 23:00:00+00:00       521   PM10  21.36  2021-01-22  23:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1277 2021-01-22 23:10:00+00:00       521    PM1  10.05  2021-01-22  23:10:00
     1278 2021-01-22 23:10:00+00:00       521  PM2.5  12.73  2021-01-22  23:10:00
     1279 2021-01-22 23:10:00+00:00       521   PM10  21.38  2021-01-22  23:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1283 2021-01-22 23:20:00+00:00       521    PM1  10.15  2021-01-22  23:20:00
     1284 2021-01-22 23:20:00+00:00       521  PM2.5  13.33  2021-01-22  23:20:00
     1285 2021-01-22 23:20:00+00:00       521   PM10  18.65  2021-01-22  23:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1289 2021-01-22 23:30:00+00:00       521    PM1   9.23  2021-01-22  23:30:00
     1290 2021-01-22 23:30:00+00:00       521  PM2.5  11.54  2021-01-22  23:30:00
     1291 2021-01-22 23:30:00+00:00       521   PM10  17.99  2021-01-22  23:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1295 2021-01-22 23:40:00+00:00       521    PM1   9.75  2021-01-22  23:40:00
     1296 2021-01-22 23:40:00+00:00       521  PM2.5  12.30  2021-01-22  23:40:00
     1297 2021-01-22 23:40:00+00:00       521   PM10  17.91  2021-01-22  23:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1301 2021-01-22 23:50:00+00:00       521    PM1   9.54  2021-01-22  23:50:00
     1302 2021-01-22 23:50:00+00:00       521  PM2.5  11.84  2021-01-22  23:50:00
     1303 2021-01-22 23:50:00+00:00       521   PM10  19.81  2021-01-22  23:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1307 2021-01-23 00:00:00+00:00       521    PM1  10.11  2021-01-23  00:00:00
     1308 2021-01-23 00:00:00+00:00       521  PM2.5  12.69  2021-01-23  00:00:00
     1309 2021-01-23 00:00:00+00:00       521   PM10  20.26  2021-01-23  00:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1313 2021-01-23 00:10:01+00:00       521    PM1   8.46  2021-01-23  00:10:01
     1314 2021-01-23 00:10:01+00:00       521  PM2.5  10.78  2021-01-23  00:10:01
     1315 2021-01-23 00:10:01+00:00       521   PM10  17.07  2021-01-23  00:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     1319 2021-01-23 00:20:00+00:00       521    PM1  10.08  2021-01-23  00:20:00
     1320 2021-01-23 00:20:00+00:00       521  PM2.5  12.88  2021-01-23  00:20:00
     1321 2021-01-23 00:20:00+00:00       521   PM10  19.55  2021-01-23  00:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1325 2021-01-23 00:30:00+00:00       521    PM1   9.24  2021-01-23  00:30:00
     1326 2021-01-23 00:30:00+00:00       521  PM2.5  11.84  2021-01-23  00:30:00
     1327 2021-01-23 00:30:00+00:00       521   PM10  18.25  2021-01-23  00:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1331 2021-01-23 00:40:00+00:00       521    PM1   9.33  2021-01-23  00:40:00
     1332 2021-01-23 00:40:00+00:00       521  PM2.5  11.51  2021-01-23  00:40:00
     1333 2021-01-23 00:40:00+00:00       521   PM10  17.68  2021-01-23  00:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1337 2021-01-23 00:50:00+00:00       521    PM1  10.34  2021-01-23  00:50:00
     1338 2021-01-23 00:50:00+00:00       521  PM2.5  13.98  2021-01-23  00:50:00
     1339 2021-01-23 00:50:00+00:00       521   PM10  17.80  2021-01-23  00:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1343 2021-01-23 01:00:00+00:00       521    PM1  10.90  2021-01-23  01:00:00
     1344 2021-01-23 01:00:00+00:00       521  PM2.5  13.11  2021-01-23  01:00:00
     1345 2021-01-23 01:00:00+00:00       521   PM10  20.30  2021-01-23  01:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1349 2021-01-23 01:10:00+00:00       521    PM1  11.29  2021-01-23  01:10:00
     1350 2021-01-23 01:10:00+00:00       521  PM2.5  14.73  2021-01-23  01:10:00
     1351 2021-01-23 01:10:00+00:00       521   PM10  20.14  2021-01-23  01:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1355 2021-01-23 01:20:00+00:00       521    PM1  10.44  2021-01-23  01:20:00
     1356 2021-01-23 01:20:00+00:00       521  PM2.5  12.06  2021-01-23  01:20:00
     1357 2021-01-23 01:20:00+00:00       521   PM10  20.51  2021-01-23  01:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1361 2021-01-23 01:30:01+00:00       521    PM1  10.36  2021-01-23  01:30:01
     1362 2021-01-23 01:30:01+00:00       521  PM2.5  12.24  2021-01-23  01:30:01
     1363 2021-01-23 01:30:01+00:00       521   PM10  17.25  2021-01-23  01:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     1367 2021-01-23 01:40:00+00:00       521    PM1  10.81  2021-01-23  01:40:00
     1368 2021-01-23 01:40:00+00:00       521  PM2.5  13.82  2021-01-23  01:40:00
     1369 2021-01-23 01:40:00+00:00       521   PM10  19.62  2021-01-23  01:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1373 2021-01-23 01:50:00+00:00       521    PM1  11.51  2021-01-23  01:50:00
     1374 2021-01-23 01:50:00+00:00       521  PM2.5  13.54  2021-01-23  01:50:00
     1375 2021-01-23 01:50:00+00:00       521   PM10  18.63  2021-01-23  01:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1379 2021-01-23 02:00:00+00:00       521    PM1  10.94  2021-01-23  02:00:00
     1380 2021-01-23 02:00:00+00:00       521  PM2.5  13.37  2021-01-23  02:00:00
     1381 2021-01-23 02:00:00+00:00       521   PM10  20.50  2021-01-23  02:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1385 2021-01-23 02:10:00+00:00       521    PM1  10.34  2021-01-23  02:10:00
     1386 2021-01-23 02:10:00+00:00       521  PM2.5  12.33  2021-01-23  02:10:00
     1387 2021-01-23 02:10:00+00:00       521   PM10  17.66  2021-01-23  02:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1391 2021-01-23 02:20:00+00:00       521    PM1  12.16  2021-01-23  02:20:00
     1392 2021-01-23 02:20:00+00:00       521  PM2.5  14.67  2021-01-23  02:20:00
     1393 2021-01-23 02:20:00+00:00       521   PM10  21.44  2021-01-23  02:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1397 2021-01-23 02:30:00+00:00       521    PM1  11.49  2021-01-23  02:30:00
     1398 2021-01-23 02:30:00+00:00       521  PM2.5  14.14  2021-01-23  02:30:00
     1399 2021-01-23 02:30:00+00:00       521   PM10  20.85  2021-01-23  02:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1403 2021-01-23 02:40:00+00:00       521    PM1  12.50  2021-01-23  02:40:00
     1404 2021-01-23 02:40:00+00:00       521  PM2.5  15.26  2021-01-23  02:40:00
     1405 2021-01-23 02:40:00+00:00       521   PM10  21.41  2021-01-23  02:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1409 2021-01-23 02:50:00+00:00       521    PM1  12.27  2021-01-23  02:50:00
     1410 2021-01-23 02:50:00+00:00       521  PM2.5  14.34  2021-01-23  02:50:00
     1411 2021-01-23 02:50:00+00:00       521   PM10  20.88  2021-01-23  02:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1415 2021-01-23 03:00:00+00:00       521    PM1  13.09  2021-01-23  03:00:00
     1416 2021-01-23 03:00:00+00:00       521  PM2.5  15.20  2021-01-23  03:00:00
     1417 2021-01-23 03:00:00+00:00       521   PM10  18.89  2021-01-23  03:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1421 2021-01-23 03:10:00+00:00       521    PM1  11.94  2021-01-23  03:10:00
     1422 2021-01-23 03:10:00+00:00       521  PM2.5  14.88  2021-01-23  03:10:00
     1423 2021-01-23 03:10:00+00:00       521   PM10  22.45  2021-01-23  03:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1427 2021-01-23 03:20:00+00:00       521    PM1  12.37  2021-01-23  03:20:00
     1428 2021-01-23 03:20:00+00:00       521  PM2.5  14.62  2021-01-23  03:20:00
     1429 2021-01-23 03:20:00+00:00       521   PM10  19.05  2021-01-23  03:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1433 2021-01-23 03:30:00+00:00       521    PM1  11.14  2021-01-23  03:30:00
     1434 2021-01-23 03:30:00+00:00       521  PM2.5  13.19  2021-01-23  03:30:00
     1435 2021-01-23 03:30:00+00:00       521   PM10  18.12  2021-01-23  03:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1442 2021-01-23 03:50:00+00:00       521    PM1  12.94  2021-01-23  03:50:00
     1443 2021-01-23 03:50:00+00:00       521  PM2.5  14.80  2021-01-23  03:50:00
     1444 2021-01-23 03:50:00+00:00       521   PM10  19.55  2021-01-23  03:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1448 2021-01-23 04:00:00+00:00       521    PM1  13.12  2021-01-23  04:00:00
     1449 2021-01-23 04:00:00+00:00       521  PM2.5  14.89  2021-01-23  04:00:00
     1450 2021-01-23 04:00:00+00:00       521   PM10  19.79  2021-01-23  04:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1454 2021-01-23 04:10:01+00:00       521    PM1  13.40  2021-01-23  04:10:01
     1455 2021-01-23 04:10:01+00:00       521  PM2.5  15.61  2021-01-23  04:10:01
     1456 2021-01-23 04:10:01+00:00       521   PM10  20.12  2021-01-23  04:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     1460 2021-01-23 04:19:59+00:00       521    PM1  13.80  2021-01-23  04:19:59
     1461 2021-01-23 04:19:59+00:00       521  PM2.5  15.65  2021-01-23  04:19:59
     1462 2021-01-23 04:19:59+00:00       521   PM10  19.73  2021-01-23  04:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     1466 2021-01-23 04:30:01+00:00       521    PM1  13.79  2021-01-23  04:30:01
     1467 2021-01-23 04:30:01+00:00       521  PM2.5  15.80  2021-01-23  04:30:01
     1468 2021-01-23 04:30:01+00:00       521   PM10  21.18  2021-01-23  04:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     1472 2021-01-23 04:40:00+00:00       521    PM1  15.58  2021-01-23  04:40:00
     1473 2021-01-23 04:40:00+00:00       521  PM2.5  17.83  2021-01-23  04:40:00
     1474 2021-01-23 04:40:00+00:00       521   PM10  23.62  2021-01-23  04:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1478 2021-01-23 04:50:00+00:00       521    PM1  15.11  2021-01-23  04:50:00
     1479 2021-01-23 04:50:00+00:00       521  PM2.5  17.74  2021-01-23  04:50:00
     1480 2021-01-23 04:50:00+00:00       521   PM10  23.11  2021-01-23  04:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1484 2021-01-23 05:00:00+00:00       521    PM1  14.84  2021-01-23  05:00:00
     1485 2021-01-23 05:00:00+00:00       521  PM2.5  17.02  2021-01-23  05:00:00
     1486 2021-01-23 05:00:00+00:00       521   PM10  20.09  2021-01-23  05:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1490 2021-01-23 05:10:01+00:00       521    PM1  13.63  2021-01-23  05:10:01
     1491 2021-01-23 05:10:01+00:00       521  PM2.5  14.77  2021-01-23  05:10:01
     1492 2021-01-23 05:10:01+00:00       521   PM10  19.30  2021-01-23  05:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     1496 2021-01-23 05:19:59+00:00       521    PM1  12.52  2021-01-23  05:19:59
     1497 2021-01-23 05:19:59+00:00       521  PM2.5  14.21  2021-01-23  05:19:59
     1498 2021-01-23 05:19:59+00:00       521   PM10  19.04  2021-01-23  05:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     1502 2021-01-23 05:30:00+00:00       521    PM1  12.00  2021-01-23  05:30:00
     1503 2021-01-23 05:30:00+00:00       521  PM2.5  13.61  2021-01-23  05:30:00
     1504 2021-01-23 05:30:00+00:00       521   PM10  17.19  2021-01-23  05:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1508 2021-01-23 05:40:00+00:00       521    PM1  11.62  2021-01-23  05:40:00
     1509 2021-01-23 05:40:00+00:00       521  PM2.5  12.60  2021-01-23  05:40:00
     1510 2021-01-23 05:40:00+00:00       521   PM10  17.44  2021-01-23  05:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1514 2021-01-23 05:50:01+00:00       521    PM1  11.37  2021-01-23  05:50:01
     1515 2021-01-23 05:50:01+00:00       521  PM2.5  12.92  2021-01-23  05:50:01
     1516 2021-01-23 05:50:01+00:00       521   PM10  18.41  2021-01-23  05:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     1520 2021-01-23 06:00:00+00:00       521    PM1   9.58  2021-01-23  06:00:00
     1521 2021-01-23 06:00:00+00:00       521  PM2.5  11.09  2021-01-23  06:00:00
     1522 2021-01-23 06:00:00+00:00       521   PM10  14.05  2021-01-23  06:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1526 2021-01-23 06:10:00+00:00       521    PM1   9.73  2021-01-23  06:10:00
     1527 2021-01-23 06:10:00+00:00       521  PM2.5  10.62  2021-01-23  06:10:00
     1528 2021-01-23 06:10:00+00:00       521   PM10  13.82  2021-01-23  06:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1532 2021-01-23 06:20:00+00:00       521    PM1   8.10  2021-01-23  06:20:00
     1533 2021-01-23 06:20:00+00:00       521  PM2.5   8.98  2021-01-23  06:20:00
     1534 2021-01-23 06:20:00+00:00       521   PM10  12.14  2021-01-23  06:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1538 2021-01-23 06:30:00+00:00       521    PM1   6.28  2021-01-23  06:30:00
     1539 2021-01-23 06:30:00+00:00       521  PM2.5   6.55  2021-01-23  06:30:00
     1540 2021-01-23 06:30:00+00:00       521   PM10   9.17  2021-01-23  06:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1544 2021-01-23 06:40:00+00:00       521    PM1   9.11  2021-01-23  06:40:00
     1545 2021-01-23 06:40:00+00:00       521  PM2.5  10.16  2021-01-23  06:40:00
     1546 2021-01-23 06:40:00+00:00       521   PM10  13.52  2021-01-23  06:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1550 2021-01-23 06:50:00+00:00       521    PM1  10.27  2021-01-23  06:50:00
     1551 2021-01-23 06:50:00+00:00       521  PM2.5  11.77  2021-01-23  06:50:00
     1552 2021-01-23 06:50:00+00:00       521   PM10  15.42  2021-01-23  06:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1556 2021-01-23 07:00:00+00:00       521    PM1   9.10  2021-01-23  07:00:00
     1557 2021-01-23 07:00:00+00:00       521  PM2.5  10.61  2021-01-23  07:00:00
     1558 2021-01-23 07:00:00+00:00       521   PM10  14.65  2021-01-23  07:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1562 2021-01-23 07:10:00+00:00       521    PM1   7.32  2021-01-23  07:10:00
     1563 2021-01-23 07:10:00+00:00       521  PM2.5   8.10  2021-01-23  07:10:00
     1564 2021-01-23 07:10:00+00:00       521   PM10  11.45  2021-01-23  07:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1568 2021-01-23 07:20:00+00:00       521    PM1   6.89  2021-01-23  07:20:00
     1569 2021-01-23 07:20:00+00:00       521  PM2.5   7.89  2021-01-23  07:20:00
     1570 2021-01-23 07:20:00+00:00       521   PM10  12.18  2021-01-23  07:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1574 2021-01-23 07:30:00+00:00       521    PM1   7.43  2021-01-23  07:30:00
     1575 2021-01-23 07:30:00+00:00       521  PM2.5   8.85  2021-01-23  07:30:00
     1576 2021-01-23 07:30:00+00:00       521   PM10  12.64  2021-01-23  07:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1580 2021-01-23 07:40:00+00:00       521    PM1   5.42  2021-01-23  07:40:00
     1581 2021-01-23 07:40:00+00:00       521  PM2.5   6.33  2021-01-23  07:40:00
     1582 2021-01-23 07:40:00+00:00       521   PM10   9.20  2021-01-23  07:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1586 2021-01-23 07:50:01+00:00       521    PM1   6.02  2021-01-23  07:50:01
     1587 2021-01-23 07:50:01+00:00       521  PM2.5   6.82  2021-01-23  07:50:01
     1588 2021-01-23 07:50:01+00:00       521   PM10  10.50  2021-01-23  07:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     1592 2021-01-23 08:00:00+00:00       521    PM1   5.87  2021-01-23  08:00:00
     1593 2021-01-23 08:00:00+00:00       521  PM2.5   7.08  2021-01-23  08:00:00
     1594 2021-01-23 08:00:00+00:00       521   PM10  12.54  2021-01-23  08:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1598 2021-01-23 08:10:00+00:00       521    PM1   4.97  2021-01-23  08:10:00
     1599 2021-01-23 08:10:00+00:00       521  PM2.5   6.69  2021-01-23  08:10:00
     1600 2021-01-23 08:10:00+00:00       521   PM10  10.41  2021-01-23  08:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1604 2021-01-23 08:20:00+00:00       521    PM1   4.53  2021-01-23  08:20:00
     1605 2021-01-23 08:20:00+00:00       521  PM2.5   5.82  2021-01-23  08:20:00
     1606 2021-01-23 08:20:00+00:00       521   PM10   8.57  2021-01-23  08:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1610 2021-01-23 08:30:00+00:00       521    PM1   4.04  2021-01-23  08:30:00
     1611 2021-01-23 08:30:00+00:00       521  PM2.5   5.48  2021-01-23  08:30:00
     1612 2021-01-23 08:30:00+00:00       521   PM10  12.77  2021-01-23  08:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1616 2021-01-23 08:40:00+00:00       521    PM1   3.39  2021-01-23  08:40:00
     1617 2021-01-23 08:40:00+00:00       521  PM2.5   4.11  2021-01-23  08:40:00
     1618 2021-01-23 08:40:00+00:00       521   PM10   8.00  2021-01-23  08:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1622 2021-01-23 08:50:00+00:00       521    PM1   3.30  2021-01-23  08:50:00
     1623 2021-01-23 08:50:00+00:00       521  PM2.5   4.02  2021-01-23  08:50:00
     1624 2021-01-23 08:50:00+00:00       521   PM10   7.52  2021-01-23  08:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1628 2021-01-23 09:00:00+00:00       521    PM1   3.14  2021-01-23  09:00:00
     1629 2021-01-23 09:00:00+00:00       521  PM2.5   4.16  2021-01-23  09:00:00
     1630 2021-01-23 09:00:00+00:00       521   PM10   8.94  2021-01-23  09:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1634 2021-01-23 09:10:00+00:00       521    PM1   3.09  2021-01-23  09:10:00
     1635 2021-01-23 09:10:00+00:00       521  PM2.5   4.03  2021-01-23  09:10:00
     1636 2021-01-23 09:10:00+00:00       521   PM10   8.93  2021-01-23  09:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1640 2021-01-23 09:20:01+00:00       521    PM1   2.85  2021-01-23  09:20:01
     1641 2021-01-23 09:20:01+00:00       521  PM2.5   4.09  2021-01-23  09:20:01
     1642 2021-01-23 09:20:01+00:00       521   PM10   7.68  2021-01-23  09:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     1646 2021-01-23 09:30:00+00:00       521    PM1   3.11  2021-01-23  09:30:00
     1647 2021-01-23 09:30:00+00:00       521  PM2.5   4.40  2021-01-23  09:30:00
     1648 2021-01-23 09:30:00+00:00       521   PM10   8.11  2021-01-23  09:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1652 2021-01-23 09:40:00+00:00       521    PM1   2.66  2021-01-23  09:40:00
     1653 2021-01-23 09:40:00+00:00       521  PM2.5   3.54  2021-01-23  09:40:00
     1654 2021-01-23 09:40:00+00:00       521   PM10   7.25  2021-01-23  09:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1658 2021-01-23 09:50:00+00:00       521    PM1   3.39  2021-01-23  09:50:00
     1659 2021-01-23 09:50:00+00:00       521  PM2.5   4.71  2021-01-23  09:50:00
     1660 2021-01-23 09:50:00+00:00       521   PM10  13.39  2021-01-23  09:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1664 2021-01-23 09:59:59+00:00       521    PM1   3.69  2021-01-23  09:59:59
     1665 2021-01-23 09:59:59+00:00       521  PM2.5   5.44  2021-01-23  09:59:59
     1666 2021-01-23 09:59:59+00:00       521   PM10  12.67  2021-01-23  09:59:59,
                          Timestamp  Location   Name  Value        Date      Time
     1670 2021-01-23 10:10:01+00:00       521    PM1   3.10  2021-01-23  10:10:01
     1671 2021-01-23 10:10:01+00:00       521  PM2.5   4.23  2021-01-23  10:10:01
     1672 2021-01-23 10:10:01+00:00       521   PM10   8.13  2021-01-23  10:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     1676 2021-01-23 10:20:00+00:00       521    PM1   3.44  2021-01-23  10:20:00
     1677 2021-01-23 10:20:00+00:00       521  PM2.5   4.08  2021-01-23  10:20:00
     1678 2021-01-23 10:20:00+00:00       521   PM10  10.35  2021-01-23  10:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1682 2021-01-23 10:30:00+00:00       521    PM1   3.37  2021-01-23  10:30:00
     1683 2021-01-23 10:30:00+00:00       521  PM2.5   4.46  2021-01-23  10:30:00
     1684 2021-01-23 10:30:00+00:00       521   PM10   8.44  2021-01-23  10:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1688 2021-01-23 10:40:00+00:00       521    PM1   4.76  2021-01-23  10:40:00
     1689 2021-01-23 10:40:00+00:00       521  PM2.5   6.36  2021-01-23  10:40:00
     1690 2021-01-23 10:40:00+00:00       521   PM10  12.59  2021-01-23  10:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1694 2021-01-23 10:50:00+00:00       521    PM1   3.49  2021-01-23  10:50:00
     1695 2021-01-23 10:50:00+00:00       521  PM2.5   5.35  2021-01-23  10:50:00
     1696 2021-01-23 10:50:00+00:00       521   PM10  10.65  2021-01-23  10:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1700 2021-01-23 11:00:01+00:00       521    PM1   4.98  2021-01-23  11:00:01
     1701 2021-01-23 11:00:01+00:00       521  PM2.5   6.51  2021-01-23  11:00:01
     1702 2021-01-23 11:00:01+00:00       521   PM10  10.45  2021-01-23  11:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     1706 2021-01-23 11:10:00+00:00       521    PM1   3.92  2021-01-23  11:10:00
     1707 2021-01-23 11:10:00+00:00       521  PM2.5   5.31  2021-01-23  11:10:00
     1708 2021-01-23 11:10:00+00:00       521   PM10  11.34  2021-01-23  11:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1712 2021-01-23 11:20:00+00:00       521    PM1   4.23  2021-01-23  11:20:00
     1713 2021-01-23 11:20:00+00:00       521  PM2.5   5.96  2021-01-23  11:20:00
     1714 2021-01-23 11:20:00+00:00       521   PM10  11.89  2021-01-23  11:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1718 2021-01-23 11:30:00+00:00       521    PM1   5.30  2021-01-23  11:30:00
     1719 2021-01-23 11:30:00+00:00       521  PM2.5   7.01  2021-01-23  11:30:00
     1720 2021-01-23 11:30:00+00:00       521   PM10  13.80  2021-01-23  11:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1724 2021-01-23 11:40:00+00:00       521    PM1   4.76  2021-01-23  11:40:00
     1725 2021-01-23 11:40:00+00:00       521  PM2.5   6.01  2021-01-23  11:40:00
     1726 2021-01-23 11:40:00+00:00       521   PM10  15.83  2021-01-23  11:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1730 2021-01-23 11:50:00+00:00       521    PM1   4.54  2021-01-23  11:50:00
     1731 2021-01-23 11:50:00+00:00       521  PM2.5   5.99  2021-01-23  11:50:00
     1732 2021-01-23 11:50:00+00:00       521   PM10  15.01  2021-01-23  11:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1736 2021-01-23 12:00:00+00:00       521    PM1   4.52  2021-01-23  12:00:00
     1737 2021-01-23 12:00:00+00:00       521  PM2.5   6.33  2021-01-23  12:00:00
     1738 2021-01-23 12:00:00+00:00       521   PM10  13.54  2021-01-23  12:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1742 2021-01-23 12:10:00+00:00       521    PM1   5.25  2021-01-23  12:10:00
     1743 2021-01-23 12:10:00+00:00       521  PM2.5   6.64  2021-01-23  12:10:00
     1744 2021-01-23 12:10:00+00:00       521   PM10  16.24  2021-01-23  12:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1748 2021-01-23 12:20:00+00:00       521    PM1   5.19  2021-01-23  12:20:00
     1749 2021-01-23 12:20:00+00:00       521  PM2.5   7.49  2021-01-23  12:20:00
     1750 2021-01-23 12:20:00+00:00       521   PM10  15.84  2021-01-23  12:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1754 2021-01-23 12:30:00+00:00       521    PM1   5.83  2021-01-23  12:30:00
     1755 2021-01-23 12:30:00+00:00       521  PM2.5   8.56  2021-01-23  12:30:00
     1756 2021-01-23 12:30:00+00:00       521   PM10  16.32  2021-01-23  12:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1760 2021-01-23 12:40:00+00:00       521    PM1   5.52  2021-01-23  12:40:00
     1761 2021-01-23 12:40:00+00:00       521  PM2.5   7.44  2021-01-23  12:40:00
     1762 2021-01-23 12:40:00+00:00       521   PM10  15.84  2021-01-23  12:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1766 2021-01-23 12:50:00+00:00       521    PM1   4.59  2021-01-23  12:50:00
     1767 2021-01-23 12:50:00+00:00       521  PM2.5   6.19  2021-01-23  12:50:00
     1768 2021-01-23 12:50:00+00:00       521   PM10  13.41  2021-01-23  12:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1772 2021-01-23 13:00:00+00:00       521    PM1   5.53  2021-01-23  13:00:00
     1773 2021-01-23 13:00:00+00:00       521  PM2.5   6.98  2021-01-23  13:00:00
     1774 2021-01-23 13:00:00+00:00       521   PM10  12.76  2021-01-23  13:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1778 2021-01-23 13:10:01+00:00       521    PM1   4.90  2021-01-23  13:10:01
     1779 2021-01-23 13:10:01+00:00       521  PM2.5   6.58  2021-01-23  13:10:01
     1780 2021-01-23 13:10:01+00:00       521   PM10  15.86  2021-01-23  13:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     1784 2021-01-23 13:19:59+00:00       521    PM1   4.98  2021-01-23  13:19:59
     1785 2021-01-23 13:19:59+00:00       521  PM2.5   6.76  2021-01-23  13:19:59
     1786 2021-01-23 13:19:59+00:00       521   PM10  13.24  2021-01-23  13:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     1790 2021-01-23 13:30:00+00:00       521    PM1   4.53  2021-01-23  13:30:00
     1791 2021-01-23 13:30:00+00:00       521  PM2.5   6.54  2021-01-23  13:30:00
     1792 2021-01-23 13:30:00+00:00       521   PM10  16.28  2021-01-23  13:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1796 2021-01-23 13:40:00+00:00       521    PM1   4.75  2021-01-23  13:40:00
     1797 2021-01-23 13:40:00+00:00       521  PM2.5   6.67  2021-01-23  13:40:00
     1798 2021-01-23 13:40:00+00:00       521   PM10  12.17  2021-01-23  13:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1802 2021-01-23 13:50:01+00:00       521    PM1   5.34  2021-01-23  13:50:01
     1803 2021-01-23 13:50:01+00:00       521  PM2.5   6.86  2021-01-23  13:50:01
     1804 2021-01-23 13:50:01+00:00       521   PM10  15.30  2021-01-23  13:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     1808 2021-01-23 14:00:00+00:00       521    PM1   6.16  2021-01-23  14:00:00
     1809 2021-01-23 14:00:00+00:00       521  PM2.5   7.68  2021-01-23  14:00:00
     1810 2021-01-23 14:00:00+00:00       521   PM10  13.49  2021-01-23  14:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1814 2021-01-23 14:10:00+00:00       521    PM1   4.63  2021-01-23  14:10:00
     1815 2021-01-23 14:10:00+00:00       521  PM2.5   6.19  2021-01-23  14:10:00
     1816 2021-01-23 14:10:00+00:00       521   PM10  13.34  2021-01-23  14:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1820 2021-01-23 14:20:00+00:00       521    PM1   4.20  2021-01-23  14:20:00
     1821 2021-01-23 14:20:00+00:00       521  PM2.5   6.49  2021-01-23  14:20:00
     1822 2021-01-23 14:20:00+00:00       521   PM10  11.72  2021-01-23  14:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1826 2021-01-23 14:30:00+00:00       521    PM1   5.21  2021-01-23  14:30:00
     1827 2021-01-23 14:30:00+00:00       521  PM2.5   6.74  2021-01-23  14:30:00
     1828 2021-01-23 14:30:00+00:00       521   PM10  11.04  2021-01-23  14:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1832 2021-01-23 14:40:00+00:00       521    PM1   4.73  2021-01-23  14:40:00
     1833 2021-01-23 14:40:00+00:00       521  PM2.5   6.22  2021-01-23  14:40:00
     1834 2021-01-23 14:40:00+00:00       521   PM10  12.54  2021-01-23  14:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1838 2021-01-23 14:50:00+00:00       521    PM1   4.93  2021-01-23  14:50:00
     1839 2021-01-23 14:50:00+00:00       521  PM2.5   6.43  2021-01-23  14:50:00
     1840 2021-01-23 14:50:00+00:00       521   PM10  12.28  2021-01-23  14:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1844 2021-01-23 15:00:01+00:00       521    PM1   4.91  2021-01-23  15:00:01
     1845 2021-01-23 15:00:01+00:00       521  PM2.5   6.33  2021-01-23  15:00:01
     1846 2021-01-23 15:00:01+00:00       521   PM10  10.28  2021-01-23  15:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     1850 2021-01-23 15:10:00+00:00       521    PM1   4.61  2021-01-23  15:10:00
     1851 2021-01-23 15:10:00+00:00       521  PM2.5   5.83  2021-01-23  15:10:00
     1852 2021-01-23 15:10:00+00:00       521   PM10  10.88  2021-01-23  15:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1856 2021-01-23 15:20:00+00:00       521    PM1   5.36  2021-01-23  15:20:00
     1857 2021-01-23 15:20:00+00:00       521  PM2.5   6.63  2021-01-23  15:20:00
     1858 2021-01-23 15:20:00+00:00       521   PM10  10.59  2021-01-23  15:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1862 2021-01-23 15:30:00+00:00       521    PM1   5.31  2021-01-23  15:30:00
     1863 2021-01-23 15:30:00+00:00       521  PM2.5   6.66  2021-01-23  15:30:00
     1864 2021-01-23 15:30:00+00:00       521   PM10  11.24  2021-01-23  15:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1868 2021-01-23 15:40:00+00:00       521    PM1   6.79  2021-01-23  15:40:00
     1869 2021-01-23 15:40:00+00:00       521  PM2.5   8.07  2021-01-23  15:40:00
     1870 2021-01-23 15:40:00+00:00       521   PM10  12.87  2021-01-23  15:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1874 2021-01-23 15:50:00+00:00       521    PM1   6.11  2021-01-23  15:50:00
     1875 2021-01-23 15:50:00+00:00       521  PM2.5   7.29  2021-01-23  15:50:00
     1876 2021-01-23 15:50:00+00:00       521   PM10  12.03  2021-01-23  15:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1880 2021-01-23 16:00:00+00:00       521    PM1   7.07  2021-01-23  16:00:00
     1881 2021-01-23 16:00:00+00:00       521  PM2.5   8.30  2021-01-23  16:00:00
     1882 2021-01-23 16:00:00+00:00       521   PM10  13.25  2021-01-23  16:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1886 2021-01-23 16:10:00+00:00       521    PM1   6.84  2021-01-23  16:10:00
     1887 2021-01-23 16:10:00+00:00       521  PM2.5   8.16  2021-01-23  16:10:00
     1888 2021-01-23 16:10:00+00:00       521   PM10  12.61  2021-01-23  16:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1892 2021-01-23 16:20:00+00:00       521    PM1   6.50  2021-01-23  16:20:00
     1893 2021-01-23 16:20:00+00:00       521  PM2.5   8.74  2021-01-23  16:20:00
     1894 2021-01-23 16:20:00+00:00       521   PM10  13.36  2021-01-23  16:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1898 2021-01-23 16:30:00+00:00       521    PM1   6.85  2021-01-23  16:30:00
     1899 2021-01-23 16:30:00+00:00       521  PM2.5   8.11  2021-01-23  16:30:00
     1900 2021-01-23 16:30:00+00:00       521   PM10  15.21  2021-01-23  16:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1904 2021-01-23 16:40:00+00:00       521    PM1   7.35  2021-01-23  16:40:00
     1905 2021-01-23 16:40:00+00:00       521  PM2.5   8.60  2021-01-23  16:40:00
     1906 2021-01-23 16:40:00+00:00       521   PM10  14.08  2021-01-23  16:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1910 2021-01-23 16:50:00+00:00       521    PM1   9.23  2021-01-23  16:50:00
     1911 2021-01-23 16:50:00+00:00       521  PM2.5  10.65  2021-01-23  16:50:00
     1912 2021-01-23 16:50:00+00:00       521   PM10  14.70  2021-01-23  16:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1916 2021-01-23 17:00:00+00:00       521    PM1  11.48  2021-01-23  17:00:00
     1917 2021-01-23 17:00:00+00:00       521  PM2.5  13.65  2021-01-23  17:00:00
     1918 2021-01-23 17:00:00+00:00       521   PM10  18.21  2021-01-23  17:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1922 2021-01-23 17:10:00+00:00       521    PM1   9.94  2021-01-23  17:10:00
     1923 2021-01-23 17:10:00+00:00       521  PM2.5  11.44  2021-01-23  17:10:00
     1924 2021-01-23 17:10:00+00:00       521   PM10  15.84  2021-01-23  17:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1928 2021-01-23 17:20:01+00:00       521    PM1  10.73  2021-01-23  17:20:01
     1929 2021-01-23 17:20:01+00:00       521  PM2.5  12.86  2021-01-23  17:20:01
     1930 2021-01-23 17:20:01+00:00       521   PM10  16.82  2021-01-23  17:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     1934 2021-01-23 17:30:00+00:00       521    PM1   9.38  2021-01-23  17:30:00
     1935 2021-01-23 17:30:00+00:00       521  PM2.5  10.75  2021-01-23  17:30:00
     1936 2021-01-23 17:30:00+00:00       521   PM10  15.45  2021-01-23  17:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1940 2021-01-23 17:40:00+00:00       521    PM1   9.00  2021-01-23  17:40:00
     1941 2021-01-23 17:40:00+00:00       521  PM2.5  10.93  2021-01-23  17:40:00
     1942 2021-01-23 17:40:00+00:00       521   PM10  14.82  2021-01-23  17:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1946 2021-01-23 17:50:00+00:00       521    PM1  10.45  2021-01-23  17:50:00
     1947 2021-01-23 17:50:00+00:00       521  PM2.5  11.73  2021-01-23  17:50:00
     1948 2021-01-23 17:50:00+00:00       521   PM10  16.20  2021-01-23  17:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1952 2021-01-23 18:00:00+00:00       521    PM1  11.64  2021-01-23  18:00:00
     1953 2021-01-23 18:00:00+00:00       521  PM2.5  13.24  2021-01-23  18:00:00
     1954 2021-01-23 18:00:00+00:00       521   PM10  18.03  2021-01-23  18:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     1958 2021-01-23 18:10:00+00:00       521    PM1  11.25  2021-01-23  18:10:00
     1959 2021-01-23 18:10:00+00:00       521  PM2.5  12.65  2021-01-23  18:10:00
     1960 2021-01-23 18:10:00+00:00       521   PM10  17.25  2021-01-23  18:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     1964 2021-01-23 18:20:00+00:00       521    PM1  12.48  2021-01-23  18:20:00
     1965 2021-01-23 18:20:00+00:00       521  PM2.5  13.53  2021-01-23  18:20:00
     1966 2021-01-23 18:20:00+00:00       521   PM10  19.09  2021-01-23  18:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     1970 2021-01-23 18:30:00+00:00       521    PM1  14.23  2021-01-23  18:30:00
     1971 2021-01-23 18:30:00+00:00       521  PM2.5  16.34  2021-01-23  18:30:00
     1972 2021-01-23 18:30:00+00:00       521   PM10  21.95  2021-01-23  18:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     1976 2021-01-23 18:40:00+00:00       521    PM1  13.71  2021-01-23  18:40:00
     1977 2021-01-23 18:40:00+00:00       521  PM2.5  15.71  2021-01-23  18:40:00
     1978 2021-01-23 18:40:00+00:00       521   PM10  21.25  2021-01-23  18:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     1982 2021-01-23 18:50:00+00:00       521    PM1  15.03  2021-01-23  18:50:00
     1983 2021-01-23 18:50:00+00:00       521  PM2.5  16.95  2021-01-23  18:50:00
     1984 2021-01-23 18:50:00+00:00       521   PM10  21.73  2021-01-23  18:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     1988 2021-01-23 19:00:01+00:00       521    PM1  14.77  2021-01-23  19:00:01
     1989 2021-01-23 19:00:01+00:00       521  PM2.5  17.61  2021-01-23  19:00:01
     1990 2021-01-23 19:00:01+00:00       521   PM10  23.57  2021-01-23  19:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     1994 2021-01-23 19:10:00+00:00       521    PM1  14.43  2021-01-23  19:10:00
     1995 2021-01-23 19:10:00+00:00       521  PM2.5  17.06  2021-01-23  19:10:00
     1996 2021-01-23 19:10:00+00:00       521   PM10  23.29  2021-01-23  19:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2000 2021-01-23 19:20:00+00:00       521    PM1  15.55  2021-01-23  19:20:00
     2001 2021-01-23 19:20:00+00:00       521  PM2.5  19.01  2021-01-23  19:20:00
     2002 2021-01-23 19:20:00+00:00       521   PM10  24.04  2021-01-23  19:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2006 2021-01-23 19:30:00+00:00       521    PM1  17.67  2021-01-23  19:30:00
     2007 2021-01-23 19:30:00+00:00       521  PM2.5  21.97  2021-01-23  19:30:00
     2008 2021-01-23 19:30:00+00:00       521   PM10  26.56  2021-01-23  19:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2012 2021-01-23 19:40:00+00:00       521    PM1  18.60  2021-01-23  19:40:00
     2013 2021-01-23 19:40:00+00:00       521  PM2.5  22.80  2021-01-23  19:40:00
     2014 2021-01-23 19:40:00+00:00       521   PM10  29.04  2021-01-23  19:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2018 2021-01-23 19:50:00+00:00       521    PM1  20.06  2021-01-23  19:50:00
     2019 2021-01-23 19:50:00+00:00       521  PM2.5  24.99  2021-01-23  19:50:00
     2020 2021-01-23 19:50:00+00:00       521   PM10  29.93  2021-01-23  19:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2024 2021-01-23 20:00:00+00:00       521    PM1  19.59  2021-01-23  20:00:00
     2025 2021-01-23 20:00:00+00:00       521  PM2.5  23.25  2021-01-23  20:00:00
     2026 2021-01-23 20:00:00+00:00       521   PM10  28.81  2021-01-23  20:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2030 2021-01-23 20:10:00+00:00       521    PM1  16.66  2021-01-23  20:10:00
     2031 2021-01-23 20:10:00+00:00       521  PM2.5  19.11  2021-01-23  20:10:00
     2032 2021-01-23 20:10:00+00:00       521   PM10  22.32  2021-01-23  20:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2036 2021-01-23 20:20:00+00:00       521    PM1  14.50  2021-01-23  20:20:00
     2037 2021-01-23 20:20:00+00:00       521  PM2.5  16.57  2021-01-23  20:20:00
     2038 2021-01-23 20:20:00+00:00       521   PM10  20.05  2021-01-23  20:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2042 2021-01-23 20:30:01+00:00       521    PM1  18.60  2021-01-23  20:30:01
     2043 2021-01-23 20:30:01+00:00       521  PM2.5  21.44  2021-01-23  20:30:01
     2044 2021-01-23 20:30:01+00:00       521   PM10  27.30  2021-01-23  20:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     2048 2021-01-23 20:40:00+00:00       521    PM1  20.62  2021-01-23  20:40:00
     2049 2021-01-23 20:40:00+00:00       521  PM2.5  23.89  2021-01-23  20:40:00
     2050 2021-01-23 20:40:00+00:00       521   PM10  26.62  2021-01-23  20:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2054 2021-01-23 20:50:00+00:00       521    PM1  18.39  2021-01-23  20:50:00
     2055 2021-01-23 20:50:00+00:00       521  PM2.5  21.20  2021-01-23  20:50:00
     2056 2021-01-23 20:50:00+00:00       521   PM10  27.31  2021-01-23  20:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2060 2021-01-23 21:00:00+00:00       521    PM1  18.77  2021-01-23  21:00:00
     2061 2021-01-23 21:00:00+00:00       521  PM2.5  21.52  2021-01-23  21:00:00
     2062 2021-01-23 21:00:00+00:00       521   PM10  25.65  2021-01-23  21:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2066 2021-01-23 21:10:00+00:00       521    PM1  19.22  2021-01-23  21:10:00
     2067 2021-01-23 21:10:00+00:00       521  PM2.5  21.76  2021-01-23  21:10:00
     2068 2021-01-23 21:10:00+00:00       521   PM10  26.63  2021-01-23  21:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2072 2021-01-23 21:20:00+00:00       521    PM1  16.88  2021-01-23  21:20:00
     2073 2021-01-23 21:20:00+00:00       521  PM2.5  19.69  2021-01-23  21:20:00
     2074 2021-01-23 21:20:00+00:00       521   PM10  24.98  2021-01-23  21:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2078 2021-01-23 21:30:00+00:00       521    PM1  17.38  2021-01-23  21:30:00
     2079 2021-01-23 21:30:00+00:00       521  PM2.5  19.21  2021-01-23  21:30:00
     2080 2021-01-23 21:30:00+00:00       521   PM10  24.92  2021-01-23  21:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2084 2021-01-23 21:40:00+00:00       521    PM1  15.78  2021-01-23  21:40:00
     2085 2021-01-23 21:40:00+00:00       521  PM2.5  17.97  2021-01-23  21:40:00
     2086 2021-01-23 21:40:00+00:00       521   PM10  21.45  2021-01-23  21:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2090 2021-01-23 21:50:00+00:00       521    PM1  15.82  2021-01-23  21:50:00
     2091 2021-01-23 21:50:00+00:00       521  PM2.5  17.97  2021-01-23  21:50:00
     2092 2021-01-23 21:50:00+00:00       521   PM10  22.20  2021-01-23  21:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2096 2021-01-23 22:00:00+00:00       521    PM1  12.65  2021-01-23  22:00:00
     2097 2021-01-23 22:00:00+00:00       521  PM2.5  14.35  2021-01-23  22:00:00
     2098 2021-01-23 22:00:00+00:00       521   PM10  18.58  2021-01-23  22:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2102 2021-01-23 22:10:00+00:00       521    PM1   9.31  2021-01-23  22:10:00
     2103 2021-01-23 22:10:00+00:00       521  PM2.5  11.32  2021-01-23  22:10:00
     2104 2021-01-23 22:10:00+00:00       521   PM10  14.38  2021-01-23  22:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2108 2021-01-23 22:20:00+00:00       521    PM1   8.78  2021-01-23  22:20:00
     2109 2021-01-23 22:20:00+00:00       521  PM2.5   9.94  2021-01-23  22:20:00
     2110 2021-01-23 22:20:00+00:00       521   PM10  13.78  2021-01-23  22:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2114 2021-01-23 22:30:00+00:00       521    PM1   7.98  2021-01-23  22:30:00
     2115 2021-01-23 22:30:00+00:00       521  PM2.5   9.44  2021-01-23  22:30:00
     2116 2021-01-23 22:30:00+00:00       521   PM10  14.16  2021-01-23  22:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2120 2021-01-23 22:40:00+00:00       521    PM1   9.44  2021-01-23  22:40:00
     2121 2021-01-23 22:40:00+00:00       521  PM2.5  10.48  2021-01-23  22:40:00
     2122 2021-01-23 22:40:00+00:00       521   PM10  14.10  2021-01-23  22:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2126 2021-01-23 22:50:01+00:00       521    PM1   9.53  2021-01-23  22:50:01
     2127 2021-01-23 22:50:01+00:00       521  PM2.5  10.66  2021-01-23  22:50:01
     2128 2021-01-23 22:50:01+00:00       521   PM10  14.70  2021-01-23  22:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     2132 2021-01-23 23:00:00+00:00       521    PM1   9.03  2021-01-23  23:00:00
     2133 2021-01-23 23:00:00+00:00       521  PM2.5  10.70  2021-01-23  23:00:00
     2134 2021-01-23 23:00:00+00:00       521   PM10  15.70  2021-01-23  23:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2138 2021-01-23 23:10:00+00:00       521    PM1  11.32  2021-01-23  23:10:00
     2139 2021-01-23 23:10:00+00:00       521  PM2.5  12.14  2021-01-23  23:10:00
     2140 2021-01-23 23:10:00+00:00       521   PM10  16.29  2021-01-23  23:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2144 2021-01-23 23:20:00+00:00       521    PM1  11.47  2021-01-23  23:20:00
     2145 2021-01-23 23:20:00+00:00       521  PM2.5  13.03  2021-01-23  23:20:00
     2146 2021-01-23 23:20:00+00:00       521   PM10  16.00  2021-01-23  23:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2150 2021-01-23 23:30:00+00:00       521    PM1  10.50  2021-01-23  23:30:00
     2151 2021-01-23 23:30:00+00:00       521  PM2.5  11.83  2021-01-23  23:30:00
     2152 2021-01-23 23:30:00+00:00       521   PM10  15.14  2021-01-23  23:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2156 2021-01-23 23:40:00+00:00       521    PM1  10.55  2021-01-23  23:40:00
     2157 2021-01-23 23:40:00+00:00       521  PM2.5  12.27  2021-01-23  23:40:00
     2158 2021-01-23 23:40:00+00:00       521   PM10  15.28  2021-01-23  23:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2162 2021-01-23 23:50:00+00:00       521    PM1   9.76  2021-01-23  23:50:00
     2163 2021-01-23 23:50:00+00:00       521  PM2.5  11.45  2021-01-23  23:50:00
     2164 2021-01-23 23:50:00+00:00       521   PM10  15.52  2021-01-23  23:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2168 2021-01-24 00:00:01+00:00       521    PM1   8.24  2021-01-24  00:00:01
     2169 2021-01-24 00:00:01+00:00       521  PM2.5   9.25  2021-01-24  00:00:01
     2170 2021-01-24 00:00:01+00:00       521   PM10  12.90  2021-01-24  00:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     2174 2021-01-24 00:10:00+00:00       521    PM1   8.56  2021-01-24  00:10:00
     2175 2021-01-24 00:10:00+00:00       521  PM2.5   9.59  2021-01-24  00:10:00
     2176 2021-01-24 00:10:00+00:00       521   PM10  12.78  2021-01-24  00:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2180 2021-01-24 00:20:00+00:00       521    PM1   7.80  2021-01-24  00:20:00
     2181 2021-01-24 00:20:00+00:00       521  PM2.5   8.68  2021-01-24  00:20:00
     2182 2021-01-24 00:20:00+00:00       521   PM10  12.32  2021-01-24  00:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2186 2021-01-24 00:30:00+00:00       521    PM1   7.10  2021-01-24  00:30:00
     2187 2021-01-24 00:30:00+00:00       521  PM2.5   8.37  2021-01-24  00:30:00
     2188 2021-01-24 00:30:00+00:00       521   PM10  10.92  2021-01-24  00:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2191 2021-01-24 00:40:00+00:00       521    PM1   5.23  2021-01-24  00:40:00
     2192 2021-01-24 00:40:00+00:00       521  PM2.5   6.32  2021-01-24  00:40:00
     2193 2021-01-24 00:40:00+00:00       521   PM10   9.31  2021-01-24  00:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2196 2021-01-24 00:50:00+00:00       521    PM1   4.40  2021-01-24  00:50:00
     2197 2021-01-24 00:50:00+00:00       521  PM2.5   5.16  2021-01-24  00:50:00
     2198 2021-01-24 00:50:00+00:00       521   PM10   9.22  2021-01-24  00:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2201 2021-01-24 01:00:00+00:00       521    PM1   3.95  2021-01-24  01:00:00
     2202 2021-01-24 01:00:00+00:00       521  PM2.5   4.57  2021-01-24  01:00:00
     2203 2021-01-24 01:00:00+00:00       521   PM10   9.21  2021-01-24  01:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2207 2021-01-24 01:10:00+00:00       521    PM1   3.87  2021-01-24  01:10:00
     2208 2021-01-24 01:10:00+00:00       521  PM2.5   4.43  2021-01-24  01:10:00
     2209 2021-01-24 01:10:00+00:00       521   PM10   7.41  2021-01-24  01:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2213 2021-01-24 01:20:00+00:00       521    PM1   3.71  2021-01-24  01:20:00
     2214 2021-01-24 01:20:00+00:00       521  PM2.5   4.31  2021-01-24  01:20:00
     2215 2021-01-24 01:20:00+00:00       521   PM10   7.22  2021-01-24  01:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2219 2021-01-24 01:30:00+00:00       521    PM1   3.43  2021-01-24  01:30:00
     2220 2021-01-24 01:30:00+00:00       521  PM2.5   4.10  2021-01-24  01:30:00
     2221 2021-01-24 01:30:00+00:00       521   PM10   6.88  2021-01-24  01:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2225 2021-01-24 01:40:00+00:00       521    PM1   3.93  2021-01-24  01:40:00
     2226 2021-01-24 01:40:00+00:00       521  PM2.5   4.33  2021-01-24  01:40:00
     2227 2021-01-24 01:40:00+00:00       521   PM10   8.87  2021-01-24  01:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2231 2021-01-24 01:50:00+00:00       521    PM1   3.58  2021-01-24  01:50:00
     2232 2021-01-24 01:50:00+00:00       521  PM2.5   4.53  2021-01-24  01:50:00
     2233 2021-01-24 01:50:00+00:00       521   PM10   7.93  2021-01-24  01:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2237 2021-01-24 02:00:00+00:00       521    PM1   3.24  2021-01-24  02:00:00
     2238 2021-01-24 02:00:00+00:00       521  PM2.5   3.85  2021-01-24  02:00:00
     2239 2021-01-24 02:00:00+00:00       521   PM10   7.18  2021-01-24  02:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2243 2021-01-24 02:10:00+00:00       521    PM1   2.78  2021-01-24  02:10:00
     2244 2021-01-24 02:10:00+00:00       521  PM2.5   3.22  2021-01-24  02:10:00
     2245 2021-01-24 02:10:00+00:00       521   PM10   6.43  2021-01-24  02:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2249 2021-01-24 02:20:00+00:00       521    PM1   2.73  2021-01-24  02:20:00
     2250 2021-01-24 02:20:00+00:00       521  PM2.5   3.65  2021-01-24  02:20:00
     2251 2021-01-24 02:20:00+00:00       521   PM10   9.14  2021-01-24  02:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2255 2021-01-24 02:30:01+00:00       521    PM1   2.86  2021-01-24  02:30:01
     2256 2021-01-24 02:30:01+00:00       521  PM2.5   3.66  2021-01-24  02:30:01
     2257 2021-01-24 02:30:01+00:00       521   PM10   6.34  2021-01-24  02:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     2261 2021-01-24 02:40:00+00:00       521    PM1   2.26  2021-01-24  02:40:00
     2262 2021-01-24 02:40:00+00:00       521  PM2.5   2.72  2021-01-24  02:40:00
     2263 2021-01-24 02:40:00+00:00       521   PM10  11.74  2021-01-24  02:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2267 2021-01-24 02:50:00+00:00       521    PM1   1.98  2021-01-24  02:50:00
     2268 2021-01-24 02:50:00+00:00       521  PM2.5   2.45  2021-01-24  02:50:00
     2269 2021-01-24 02:50:00+00:00       521   PM10   6.10  2021-01-24  02:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2273 2021-01-24 03:00:00+00:00       521    PM1   1.78  2021-01-24  03:00:00
     2274 2021-01-24 03:00:00+00:00       521  PM2.5   1.94  2021-01-24  03:00:00
     2275 2021-01-24 03:00:00+00:00       521   PM10   3.96  2021-01-24  03:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2279 2021-01-24 03:10:00+00:00       521    PM1   1.49  2021-01-24  03:10:00
     2280 2021-01-24 03:10:00+00:00       521  PM2.5   1.64  2021-01-24  03:10:00
     2281 2021-01-24 03:10:00+00:00       521   PM10   4.00  2021-01-24  03:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2285 2021-01-24 03:20:00+00:00       521    PM1   1.34  2021-01-24  03:20:00
     2286 2021-01-24 03:20:00+00:00       521  PM2.5   1.40  2021-01-24  03:20:00
     2287 2021-01-24 03:20:00+00:00       521   PM10   3.45  2021-01-24  03:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2291 2021-01-24 03:30:00+00:00       521    PM1   1.55  2021-01-24  03:30:00
     2292 2021-01-24 03:30:00+00:00       521  PM2.5   1.73  2021-01-24  03:30:00
     2293 2021-01-24 03:30:00+00:00       521   PM10   3.87  2021-01-24  03:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2300 2021-01-24 03:50:00+00:00       521    PM1   1.73  2021-01-24  03:50:00
     2301 2021-01-24 03:50:00+00:00       521  PM2.5   2.17  2021-01-24  03:50:00
     2302 2021-01-24 03:50:00+00:00       521   PM10   4.44  2021-01-24  03:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2306 2021-01-24 04:00:01+00:00       521    PM1   2.08  2021-01-24  04:00:01
     2307 2021-01-24 04:00:01+00:00       521  PM2.5   2.34  2021-01-24  04:00:01
     2308 2021-01-24 04:00:01+00:00       521   PM10   4.42  2021-01-24  04:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     2312 2021-01-24 04:10:00+00:00       521    PM1   2.15  2021-01-24  04:10:00
     2313 2021-01-24 04:10:00+00:00       521  PM2.5   2.43  2021-01-24  04:10:00
     2314 2021-01-24 04:10:00+00:00       521   PM10   4.07  2021-01-24  04:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2318 2021-01-24 04:20:00+00:00       521    PM1   1.86  2021-01-24  04:20:00
     2319 2021-01-24 04:20:00+00:00       521  PM2.5   2.33  2021-01-24  04:20:00
     2320 2021-01-24 04:20:00+00:00       521   PM10   4.31  2021-01-24  04:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2324 2021-01-24 04:30:00+00:00       521    PM1   1.82  2021-01-24  04:30:00
     2325 2021-01-24 04:30:00+00:00       521  PM2.5   2.10  2021-01-24  04:30:00
     2326 2021-01-24 04:30:00+00:00       521   PM10   4.68  2021-01-24  04:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2330 2021-01-24 04:40:00+00:00       521    PM1   1.79  2021-01-24  04:40:00
     2331 2021-01-24 04:40:00+00:00       521  PM2.5   2.24  2021-01-24  04:40:00
     2332 2021-01-24 04:40:00+00:00       521   PM10   5.07  2021-01-24  04:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2336 2021-01-24 04:50:00+00:00       521    PM1   2.37  2021-01-24  04:50:00
     2337 2021-01-24 04:50:00+00:00       521  PM2.5   3.10  2021-01-24  04:50:00
     2338 2021-01-24 04:50:00+00:00       521   PM10   7.08  2021-01-24  04:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2342 2021-01-24 05:00:00+00:00       521    PM1   2.79  2021-01-24  05:00:00
     2343 2021-01-24 05:00:00+00:00       521  PM2.5   4.13  2021-01-24  05:00:00
     2344 2021-01-24 05:00:00+00:00       521   PM10   6.56  2021-01-24  05:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2348 2021-01-24 05:10:00+00:00       521    PM1   2.22  2021-01-24  05:10:00
     2349 2021-01-24 05:10:00+00:00       521  PM2.5   2.87  2021-01-24  05:10:00
     2350 2021-01-24 05:10:00+00:00       521   PM10   9.08  2021-01-24  05:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2354 2021-01-24 05:20:00+00:00       521    PM1   2.64  2021-01-24  05:20:00
     2355 2021-01-24 05:20:00+00:00       521  PM2.5   3.67  2021-01-24  05:20:00
     2356 2021-01-24 05:20:00+00:00       521   PM10   6.81  2021-01-24  05:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2360 2021-01-24 05:30:00+00:00       521    PM1   2.58  2021-01-24  05:30:00
     2361 2021-01-24 05:30:00+00:00       521  PM2.5   3.58  2021-01-24  05:30:00
     2362 2021-01-24 05:30:00+00:00       521   PM10   8.94  2021-01-24  05:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2366 2021-01-24 05:40:00+00:00       521    PM1   2.32  2021-01-24  05:40:00
     2367 2021-01-24 05:40:00+00:00       521  PM2.5   2.75  2021-01-24  05:40:00
     2368 2021-01-24 05:40:00+00:00       521   PM10   7.14  2021-01-24  05:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2372 2021-01-24 05:50:00+00:00       521    PM1   2.35  2021-01-24  05:50:00
     2373 2021-01-24 05:50:00+00:00       521  PM2.5   2.98  2021-01-24  05:50:00
     2374 2021-01-24 05:50:00+00:00       521   PM10   6.64  2021-01-24  05:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2378 2021-01-24 06:00:01+00:00       521    PM1   2.63  2021-01-24  06:00:01
     2379 2021-01-24 06:00:01+00:00       521  PM2.5   3.50  2021-01-24  06:00:01
     2380 2021-01-24 06:00:01+00:00       521   PM10   6.86  2021-01-24  06:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     2384 2021-01-24 06:10:00+00:00       521    PM1   1.92  2021-01-24  06:10:00
     2385 2021-01-24 06:10:00+00:00       521  PM2.5   2.64  2021-01-24  06:10:00
     2386 2021-01-24 06:10:00+00:00       521   PM10   5.82  2021-01-24  06:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2390 2021-01-24 06:20:00+00:00       521    PM1   2.55  2021-01-24  06:20:00
     2391 2021-01-24 06:20:00+00:00       521  PM2.5   3.38  2021-01-24  06:20:00
     2392 2021-01-24 06:20:00+00:00       521   PM10   6.59  2021-01-24  06:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2396 2021-01-24 06:30:00+00:00       521    PM1   2.56  2021-01-24  06:30:00
     2397 2021-01-24 06:30:00+00:00       521  PM2.5   3.23  2021-01-24  06:30:00
     2398 2021-01-24 06:30:00+00:00       521   PM10   7.91  2021-01-24  06:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2402 2021-01-24 06:40:00+00:00       521    PM1   2.04  2021-01-24  06:40:00
     2403 2021-01-24 06:40:00+00:00       521  PM2.5   2.83  2021-01-24  06:40:00
     2404 2021-01-24 06:40:00+00:00       521   PM10   5.62  2021-01-24  06:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2408 2021-01-24 06:50:00+00:00       521    PM1   1.99  2021-01-24  06:50:00
     2409 2021-01-24 06:50:00+00:00       521  PM2.5   2.59  2021-01-24  06:50:00
     2410 2021-01-24 06:50:00+00:00       521   PM10   6.33  2021-01-24  06:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2414 2021-01-24 07:00:00+00:00       521    PM1   2.44  2021-01-24  07:00:00
     2415 2021-01-24 07:00:00+00:00       521  PM2.5   3.20  2021-01-24  07:00:00
     2416 2021-01-24 07:00:00+00:00       521   PM10   5.92  2021-01-24  07:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2420 2021-01-24 07:10:00+00:00       521    PM1   2.42  2021-01-24  07:10:00
     2421 2021-01-24 07:10:00+00:00       521  PM2.5   3.10  2021-01-24  07:10:00
     2422 2021-01-24 07:10:00+00:00       521   PM10   5.42  2021-01-24  07:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2426 2021-01-24 07:20:00+00:00       521    PM1   2.07  2021-01-24  07:20:00
     2427 2021-01-24 07:20:00+00:00       521  PM2.5   2.35  2021-01-24  07:20:00
     2428 2021-01-24 07:20:00+00:00       521   PM10   4.95  2021-01-24  07:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2432 2021-01-24 07:30:00+00:00       521    PM1   2.29  2021-01-24  07:30:00
     2433 2021-01-24 07:30:00+00:00       521  PM2.5   2.88  2021-01-24  07:30:00
     2434 2021-01-24 07:30:00+00:00       521   PM10   5.87  2021-01-24  07:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2438 2021-01-24 07:40:00+00:00       521    PM1   2.68  2021-01-24  07:40:00
     2439 2021-01-24 07:40:00+00:00       521  PM2.5   3.39  2021-01-24  07:40:00
     2440 2021-01-24 07:40:00+00:00       521   PM10   6.68  2021-01-24  07:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2444 2021-01-24 07:50:00+00:00       521    PM1   2.81  2021-01-24  07:50:00
     2445 2021-01-24 07:50:00+00:00       521  PM2.5   3.42  2021-01-24  07:50:00
     2446 2021-01-24 07:50:00+00:00       521   PM10   5.98  2021-01-24  07:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2450 2021-01-24 08:00:00+00:00       521    PM1   3.25  2021-01-24  08:00:00
     2451 2021-01-24 08:00:00+00:00       521  PM2.5   3.46  2021-01-24  08:00:00
     2452 2021-01-24 08:00:00+00:00       521   PM10   5.83  2021-01-24  08:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2456 2021-01-24 08:10:01+00:00       521    PM1   3.08  2021-01-24  08:10:01
     2457 2021-01-24 08:10:01+00:00       521  PM2.5   3.58  2021-01-24  08:10:01
     2458 2021-01-24 08:10:01+00:00       521   PM10   7.72  2021-01-24  08:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     2462 2021-01-24 08:20:00+00:00       521    PM1   2.64  2021-01-24  08:20:00
     2463 2021-01-24 08:20:00+00:00       521  PM2.5   3.22  2021-01-24  08:20:00
     2464 2021-01-24 08:20:00+00:00       521   PM10   5.35  2021-01-24  08:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2468 2021-01-24 08:30:00+00:00       521    PM1   2.89  2021-01-24  08:30:00
     2469 2021-01-24 08:30:00+00:00       521  PM2.5   3.35  2021-01-24  08:30:00
     2470 2021-01-24 08:30:00+00:00       521   PM10   6.21  2021-01-24  08:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2474 2021-01-24 08:40:00+00:00       521    PM1   2.90  2021-01-24  08:40:00
     2475 2021-01-24 08:40:00+00:00       521  PM2.5   3.91  2021-01-24  08:40:00
     2476 2021-01-24 08:40:00+00:00       521   PM10   8.18  2021-01-24  08:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2480 2021-01-24 08:50:00+00:00       521    PM1   3.23  2021-01-24  08:50:00
     2481 2021-01-24 08:50:00+00:00       521  PM2.5   4.14  2021-01-24  08:50:00
     2482 2021-01-24 08:50:00+00:00       521   PM10   6.98  2021-01-24  08:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2486 2021-01-24 09:00:00+00:00       521    PM1   2.33  2021-01-24  09:00:00
     2487 2021-01-24 09:00:00+00:00       521  PM2.5   3.00  2021-01-24  09:00:00
     2488 2021-01-24 09:00:00+00:00       521   PM10   7.02  2021-01-24  09:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2492 2021-01-24 09:10:01+00:00       521    PM1   2.43  2021-01-24  09:10:01
     2493 2021-01-24 09:10:01+00:00       521  PM2.5   2.97  2021-01-24  09:10:01
     2494 2021-01-24 09:10:01+00:00       521   PM10   5.55  2021-01-24  09:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     2498 2021-01-24 09:19:59+00:00       521    PM1   2.71  2021-01-24  09:19:59
     2499 2021-01-24 09:19:59+00:00       521  PM2.5   3.68  2021-01-24  09:19:59
     2500 2021-01-24 09:19:59+00:00       521   PM10   6.34  2021-01-24  09:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     2504 2021-01-24 09:30:00+00:00       521    PM1   1.91  2021-01-24  09:30:00
     2505 2021-01-24 09:30:00+00:00       521  PM2.5   2.08  2021-01-24  09:30:00
     2506 2021-01-24 09:30:00+00:00       521   PM10   4.64  2021-01-24  09:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2510 2021-01-24 09:40:01+00:00       521    PM1   2.06  2021-01-24  09:40:01
     2511 2021-01-24 09:40:01+00:00       521  PM2.5   2.67  2021-01-24  09:40:01
     2512 2021-01-24 09:40:01+00:00       521   PM10   6.92  2021-01-24  09:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     2516 2021-01-24 09:50:00+00:00       521    PM1   2.45  2021-01-24  09:50:00
     2517 2021-01-24 09:50:00+00:00       521  PM2.5   2.72  2021-01-24  09:50:00
     2518 2021-01-24 09:50:00+00:00       521   PM10   5.90  2021-01-24  09:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2522 2021-01-24 10:00:00+00:00       521    PM1   2.33  2021-01-24  10:00:00
     2523 2021-01-24 10:00:00+00:00       521  PM2.5   2.73  2021-01-24  10:00:00
     2524 2021-01-24 10:00:00+00:00       521   PM10   5.44  2021-01-24  10:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2528 2021-01-24 10:10:00+00:00       521    PM1   2.31  2021-01-24  10:10:00
     2529 2021-01-24 10:10:00+00:00       521  PM2.5   2.74  2021-01-24  10:10:00
     2530 2021-01-24 10:10:00+00:00       521   PM10   6.63  2021-01-24  10:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2534 2021-01-24 10:20:00+00:00       521    PM1   2.58  2021-01-24  10:20:00
     2535 2021-01-24 10:20:00+00:00       521  PM2.5   3.06  2021-01-24  10:20:00
     2536 2021-01-24 10:20:00+00:00       521   PM10   6.33  2021-01-24  10:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2540 2021-01-24 10:30:00+00:00       521    PM1   2.74  2021-01-24  10:30:00
     2541 2021-01-24 10:30:00+00:00       521  PM2.5   3.32  2021-01-24  10:30:00
     2542 2021-01-24 10:30:00+00:00       521   PM10   6.72  2021-01-24  10:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2546 2021-01-24 10:40:00+00:00       521    PM1   2.69  2021-01-24  10:40:00
     2547 2021-01-24 10:40:00+00:00       521  PM2.5   3.62  2021-01-24  10:40:00
     2548 2021-01-24 10:40:00+00:00       521   PM10   6.63  2021-01-24  10:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2552 2021-01-24 10:50:00+00:00       521    PM1   2.70  2021-01-24  10:50:00
     2553 2021-01-24 10:50:00+00:00       521  PM2.5   3.73  2021-01-24  10:50:00
     2554 2021-01-24 10:50:00+00:00       521   PM10   6.92  2021-01-24  10:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2558 2021-01-24 11:00:00+00:00       521    PM1   3.00  2021-01-24  11:00:00
     2559 2021-01-24 11:00:00+00:00       521  PM2.5   4.13  2021-01-24  11:00:00
     2560 2021-01-24 11:00:00+00:00       521   PM10   8.78  2021-01-24  11:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2564 2021-01-24 11:10:00+00:00       521    PM1   3.42  2021-01-24  11:10:00
     2565 2021-01-24 11:10:00+00:00       521  PM2.5   4.40  2021-01-24  11:10:00
     2566 2021-01-24 11:10:00+00:00       521   PM10  10.39  2021-01-24  11:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2570 2021-01-24 11:20:00+00:00       521    PM1   4.02  2021-01-24  11:20:00
     2571 2021-01-24 11:20:00+00:00       521  PM2.5   5.04  2021-01-24  11:20:00
     2572 2021-01-24 11:20:00+00:00       521   PM10  10.84  2021-01-24  11:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2576 2021-01-24 11:30:00+00:00       521    PM1   3.35  2021-01-24  11:30:00
     2577 2021-01-24 11:30:00+00:00       521  PM2.5   4.49  2021-01-24  11:30:00
     2578 2021-01-24 11:30:00+00:00       521   PM10  10.55  2021-01-24  11:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2582 2021-01-24 11:40:00+00:00       521    PM1   3.10  2021-01-24  11:40:00
     2583 2021-01-24 11:40:00+00:00       521  PM2.5   4.67  2021-01-24  11:40:00
     2584 2021-01-24 11:40:00+00:00       521   PM10   9.96  2021-01-24  11:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2588 2021-01-24 11:50:00+00:00       521    PM1   3.52  2021-01-24  11:50:00
     2589 2021-01-24 11:50:00+00:00       521  PM2.5   5.27  2021-01-24  11:50:00
     2590 2021-01-24 11:50:00+00:00       521   PM10  10.48  2021-01-24  11:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2594 2021-01-24 12:00:01+00:00       521    PM1   3.57  2021-01-24  12:00:01
     2595 2021-01-24 12:00:01+00:00       521  PM2.5   4.30  2021-01-24  12:00:01
     2596 2021-01-24 12:00:01+00:00       521   PM10  10.26  2021-01-24  12:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     2600 2021-01-24 12:09:59+00:00       521    PM1   3.03  2021-01-24  12:09:59
     2601 2021-01-24 12:09:59+00:00       521  PM2.5   4.18  2021-01-24  12:09:59
     2602 2021-01-24 12:09:59+00:00       521   PM10   8.23  2021-01-24  12:09:59,
                          Timestamp  Location   Name  Value        Date      Time
     2606 2021-01-24 12:20:01+00:00       521    PM1   2.99  2021-01-24  12:20:01
     2607 2021-01-24 12:20:01+00:00       521  PM2.5   4.25  2021-01-24  12:20:01
     2608 2021-01-24 12:20:01+00:00       521   PM10  10.54  2021-01-24  12:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     2612 2021-01-24 12:30:00+00:00       521    PM1   3.23  2021-01-24  12:30:00
     2613 2021-01-24 12:30:00+00:00       521  PM2.5   3.62  2021-01-24  12:30:00
     2614 2021-01-24 12:30:00+00:00       521   PM10   9.80  2021-01-24  12:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2618 2021-01-24 12:40:00+00:00       521    PM1   3.37  2021-01-24  12:40:00
     2619 2021-01-24 12:40:00+00:00       521  PM2.5   4.74  2021-01-24  12:40:00
     2620 2021-01-24 12:40:00+00:00       521   PM10   9.50  2021-01-24  12:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2624 2021-01-24 12:50:00+00:00       521    PM1   4.19  2021-01-24  12:50:00
     2625 2021-01-24 12:50:00+00:00       521  PM2.5   5.89  2021-01-24  12:50:00
     2626 2021-01-24 12:50:00+00:00       521   PM10   9.88  2021-01-24  12:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2630 2021-01-24 13:00:00+00:00       521    PM1   3.33  2021-01-24  13:00:00
     2631 2021-01-24 13:00:00+00:00       521  PM2.5   4.88  2021-01-24  13:00:00
     2632 2021-01-24 13:00:00+00:00       521   PM10   8.63  2021-01-24  13:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2636 2021-01-24 13:10:00+00:00       521    PM1   3.69  2021-01-24  13:10:00
     2637 2021-01-24 13:10:00+00:00       521  PM2.5   4.65  2021-01-24  13:10:00
     2638 2021-01-24 13:10:00+00:00       521   PM10   9.38  2021-01-24  13:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2642 2021-01-24 13:20:01+00:00       521    PM1   3.15  2021-01-24  13:20:01
     2643 2021-01-24 13:20:01+00:00       521  PM2.5   3.87  2021-01-24  13:20:01
     2644 2021-01-24 13:20:01+00:00       521   PM10   8.42  2021-01-24  13:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     2648 2021-01-24 13:30:00+00:00       521    PM1   3.08  2021-01-24  13:30:00
     2649 2021-01-24 13:30:00+00:00       521  PM2.5   3.97  2021-01-24  13:30:00
     2650 2021-01-24 13:30:00+00:00       521   PM10   9.09  2021-01-24  13:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2654 2021-01-24 13:40:00+00:00       521    PM1   7.21  2021-01-24  13:40:00
     2655 2021-01-24 13:40:00+00:00       521  PM2.5   8.54  2021-01-24  13:40:00
     2656 2021-01-24 13:40:00+00:00       521   PM10  13.70  2021-01-24  13:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2660 2021-01-24 13:50:00+00:00       521    PM1   4.60  2021-01-24  13:50:00
     2661 2021-01-24 13:50:00+00:00       521  PM2.5   5.56  2021-01-24  13:50:00
     2662 2021-01-24 13:50:00+00:00       521   PM10   9.03  2021-01-24  13:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2666 2021-01-24 14:00:00+00:00       521    PM1   5.22  2021-01-24  14:00:00
     2667 2021-01-24 14:00:00+00:00       521  PM2.5   6.37  2021-01-24  14:00:00
     2668 2021-01-24 14:00:00+00:00       521   PM10  11.04  2021-01-24  14:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2672 2021-01-24 14:10:00+00:00       521    PM1   4.36  2021-01-24  14:10:00
     2673 2021-01-24 14:10:00+00:00       521  PM2.5   5.59  2021-01-24  14:10:00
     2674 2021-01-24 14:10:00+00:00       521   PM10   8.92  2021-01-24  14:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2678 2021-01-24 14:20:00+00:00       521    PM1   7.29  2021-01-24  14:20:00
     2679 2021-01-24 14:20:00+00:00       521  PM2.5   8.64  2021-01-24  14:20:00
     2680 2021-01-24 14:20:00+00:00       521   PM10  13.87  2021-01-24  14:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2684 2021-01-24 14:30:00+00:00       521    PM1  14.07  2021-01-24  14:30:00
     2685 2021-01-24 14:30:00+00:00       521  PM2.5  17.67  2021-01-24  14:30:00
     2686 2021-01-24 14:30:00+00:00       521   PM10  22.57  2021-01-24  14:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2690 2021-01-24 14:40:00+00:00       521    PM1  14.67  2021-01-24  14:40:00
     2691 2021-01-24 14:40:00+00:00       521  PM2.5  16.95  2021-01-24  14:40:00
     2692 2021-01-24 14:40:00+00:00       521   PM10  21.85  2021-01-24  14:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2696 2021-01-24 14:50:00+00:00       521    PM1  19.03  2021-01-24  14:50:00
     2697 2021-01-24 14:50:00+00:00       521  PM2.5  25.56  2021-01-24  14:50:00
     2698 2021-01-24 14:50:00+00:00       521   PM10  30.40  2021-01-24  14:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2702 2021-01-24 15:00:00+00:00       521    PM1  28.82  2021-01-24  15:00:00
     2703 2021-01-24 15:00:00+00:00       521  PM2.5  40.56  2021-01-24  15:00:00
     2704 2021-01-24 15:00:00+00:00       521   PM10  47.43  2021-01-24  15:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2708 2021-01-24 15:10:00+00:00       521    PM1  19.19  2021-01-24  15:10:00
     2709 2021-01-24 15:10:00+00:00       521  PM2.5  22.95  2021-01-24  15:10:00
     2710 2021-01-24 15:10:00+00:00       521   PM10  27.85  2021-01-24  15:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2714 2021-01-24 15:20:00+00:00       521    PM1  15.90  2021-01-24  15:20:00
     2715 2021-01-24 15:20:00+00:00       521  PM2.5  19.12  2021-01-24  15:20:00
     2716 2021-01-24 15:20:00+00:00       521   PM10  23.31  2021-01-24  15:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2720 2021-01-24 15:30:01+00:00       521    PM1  15.43  2021-01-24  15:30:01
     2721 2021-01-24 15:30:01+00:00       521  PM2.5  18.10  2021-01-24  15:30:01
     2722 2021-01-24 15:30:01+00:00       521   PM10  22.39  2021-01-24  15:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     2726 2021-01-24 15:40:00+00:00       521    PM1   9.94  2021-01-24  15:40:00
     2727 2021-01-24 15:40:00+00:00       521  PM2.5  12.10  2021-01-24  15:40:00
     2728 2021-01-24 15:40:00+00:00       521   PM10  14.84  2021-01-24  15:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2732 2021-01-24 15:50:00+00:00       521    PM1   6.15  2021-01-24  15:50:00
     2733 2021-01-24 15:50:00+00:00       521  PM2.5   7.16  2021-01-24  15:50:00
     2734 2021-01-24 15:50:00+00:00       521   PM10  11.79  2021-01-24  15:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2738 2021-01-24 16:00:00+00:00       521    PM1   5.89  2021-01-24  16:00:00
     2739 2021-01-24 16:00:00+00:00       521  PM2.5   6.83  2021-01-24  16:00:00
     2740 2021-01-24 16:00:00+00:00       521   PM10  11.52  2021-01-24  16:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2744 2021-01-24 16:10:00+00:00       521    PM1   4.79  2021-01-24  16:10:00
     2745 2021-01-24 16:10:00+00:00       521  PM2.5   5.68  2021-01-24  16:10:00
     2746 2021-01-24 16:10:00+00:00       521   PM10   8.94  2021-01-24  16:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2750 2021-01-24 16:20:00+00:00       521    PM1   5.38  2021-01-24  16:20:00
     2751 2021-01-24 16:20:00+00:00       521  PM2.5   6.60  2021-01-24  16:20:00
     2752 2021-01-24 16:20:00+00:00       521   PM10   9.68  2021-01-24  16:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2756 2021-01-24 16:30:00+00:00       521    PM1   4.83  2021-01-24  16:30:00
     2757 2021-01-24 16:30:00+00:00       521  PM2.5   5.74  2021-01-24  16:30:00
     2758 2021-01-24 16:30:00+00:00       521   PM10   9.06  2021-01-24  16:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2762 2021-01-24 16:40:00+00:00       521    PM1   5.45  2021-01-24  16:40:00
     2763 2021-01-24 16:40:00+00:00       521  PM2.5   6.50  2021-01-24  16:40:00
     2764 2021-01-24 16:40:00+00:00       521   PM10   9.58  2021-01-24  16:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2768 2021-01-24 16:50:00+00:00       521    PM1   4.96  2021-01-24  16:50:00
     2769 2021-01-24 16:50:00+00:00       521  PM2.5   5.77  2021-01-24  16:50:00
     2770 2021-01-24 16:50:00+00:00       521   PM10   9.69  2021-01-24  16:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2774 2021-01-24 17:00:00+00:00       521    PM1   4.92  2021-01-24  17:00:00
     2775 2021-01-24 17:00:00+00:00       521  PM2.5   5.83  2021-01-24  17:00:00
     2776 2021-01-24 17:00:00+00:00       521   PM10  10.15  2021-01-24  17:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2780 2021-01-24 17:10:00+00:00       521    PM1   5.55  2021-01-24  17:10:00
     2781 2021-01-24 17:10:00+00:00       521  PM2.5   6.35  2021-01-24  17:10:00
     2782 2021-01-24 17:10:00+00:00       521   PM10  11.08  2021-01-24  17:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2786 2021-01-24 17:20:01+00:00       521    PM1   6.01  2021-01-24  17:20:01
     2787 2021-01-24 17:20:01+00:00       521  PM2.5   6.43  2021-01-24  17:20:01
     2788 2021-01-24 17:20:01+00:00       521   PM10  10.08  2021-01-24  17:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     2792 2021-01-24 17:29:59+00:00       521    PM1   5.00  2021-01-24  17:29:59
     2793 2021-01-24 17:29:59+00:00       521  PM2.5   5.68  2021-01-24  17:29:59
     2794 2021-01-24 17:29:59+00:00       521   PM10  11.03  2021-01-24  17:29:59,
                          Timestamp  Location   Name  Value        Date      Time
     2798 2021-01-24 17:40:01+00:00       521    PM1   5.75  2021-01-24  17:40:01
     2799 2021-01-24 17:40:01+00:00       521  PM2.5   6.97  2021-01-24  17:40:01
     2800 2021-01-24 17:40:01+00:00       521   PM10  10.62  2021-01-24  17:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     2804 2021-01-24 17:50:00+00:00       521    PM1   6.95  2021-01-24  17:50:00
     2805 2021-01-24 17:50:00+00:00       521  PM2.5   7.69  2021-01-24  17:50:00
     2806 2021-01-24 17:50:00+00:00       521   PM10  11.79  2021-01-24  17:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2810 2021-01-24 18:00:00+00:00       521    PM1   8.20  2021-01-24  18:00:00
     2811 2021-01-24 18:00:00+00:00       521  PM2.5   9.60  2021-01-24  18:00:00
     2812 2021-01-24 18:00:00+00:00       521   PM10  12.96  2021-01-24  18:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2816 2021-01-24 18:10:00+00:00       521    PM1   9.10  2021-01-24  18:10:00
     2817 2021-01-24 18:10:00+00:00       521  PM2.5  10.33  2021-01-24  18:10:00
     2818 2021-01-24 18:10:00+00:00       521   PM10  14.79  2021-01-24  18:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2822 2021-01-24 18:20:00+00:00       521    PM1  10.97  2021-01-24  18:20:00
     2823 2021-01-24 18:20:00+00:00       521  PM2.5  12.90  2021-01-24  18:20:00
     2824 2021-01-24 18:20:00+00:00       521   PM10  17.96  2021-01-24  18:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2828 2021-01-24 18:30:00+00:00       521    PM1   8.25  2021-01-24  18:30:00
     2829 2021-01-24 18:30:00+00:00       521  PM2.5   9.13  2021-01-24  18:30:00
     2830 2021-01-24 18:30:00+00:00       521   PM10  13.13  2021-01-24  18:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2834 2021-01-24 18:40:00+00:00       521    PM1   8.84  2021-01-24  18:40:00
     2835 2021-01-24 18:40:00+00:00       521  PM2.5   9.65  2021-01-24  18:40:00
     2836 2021-01-24 18:40:00+00:00       521   PM10  14.20  2021-01-24  18:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2840 2021-01-24 18:50:00+00:00       521    PM1  10.14  2021-01-24  18:50:00
     2841 2021-01-24 18:50:00+00:00       521  PM2.5  11.31  2021-01-24  18:50:00
     2842 2021-01-24 18:50:00+00:00       521   PM10  15.65  2021-01-24  18:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2846 2021-01-24 19:00:01+00:00       521    PM1  10.92  2021-01-24  19:00:01
     2847 2021-01-24 19:00:01+00:00       521  PM2.5  12.43  2021-01-24  19:00:01
     2848 2021-01-24 19:00:01+00:00       521   PM10  15.22  2021-01-24  19:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     2852 2021-01-24 19:10:00+00:00       521    PM1  11.82  2021-01-24  19:10:00
     2853 2021-01-24 19:10:00+00:00       521  PM2.5  14.01  2021-01-24  19:10:00
     2854 2021-01-24 19:10:00+00:00       521   PM10  16.72  2021-01-24  19:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2858 2021-01-24 19:20:00+00:00       521    PM1  12.87  2021-01-24  19:20:00
     2859 2021-01-24 19:20:00+00:00       521  PM2.5  14.26  2021-01-24  19:20:00
     2860 2021-01-24 19:20:00+00:00       521   PM10  19.15  2021-01-24  19:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2864 2021-01-24 19:30:00+00:00       521    PM1  12.02  2021-01-24  19:30:00
     2865 2021-01-24 19:30:00+00:00       521  PM2.5  13.64  2021-01-24  19:30:00
     2866 2021-01-24 19:30:00+00:00       521   PM10  18.05  2021-01-24  19:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2870 2021-01-24 19:40:00+00:00       521    PM1  13.95  2021-01-24  19:40:00
     2871 2021-01-24 19:40:00+00:00       521  PM2.5  15.58  2021-01-24  19:40:00
     2872 2021-01-24 19:40:00+00:00       521   PM10  20.57  2021-01-24  19:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2876 2021-01-24 19:50:00+00:00       521    PM1  15.02  2021-01-24  19:50:00
     2877 2021-01-24 19:50:00+00:00       521  PM2.5  16.05  2021-01-24  19:50:00
     2878 2021-01-24 19:50:00+00:00       521   PM10  20.50  2021-01-24  19:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2882 2021-01-24 20:00:00+00:00       521    PM1  17.13  2021-01-24  20:00:00
     2883 2021-01-24 20:00:00+00:00       521  PM2.5  19.00  2021-01-24  20:00:00
     2884 2021-01-24 20:00:00+00:00       521   PM10  23.61  2021-01-24  20:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2888 2021-01-24 20:10:00+00:00       521    PM1  20.04  2021-01-24  20:10:00
     2889 2021-01-24 20:10:00+00:00       521  PM2.5  23.24  2021-01-24  20:10:00
     2890 2021-01-24 20:10:00+00:00       521   PM10  30.76  2021-01-24  20:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2894 2021-01-24 20:20:00+00:00       521    PM1  17.49  2021-01-24  20:20:00
     2895 2021-01-24 20:20:00+00:00       521  PM2.5  19.94  2021-01-24  20:20:00
     2896 2021-01-24 20:20:00+00:00       521   PM10  24.97  2021-01-24  20:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2900 2021-01-24 20:30:00+00:00       521    PM1  15.11  2021-01-24  20:30:00
     2901 2021-01-24 20:30:00+00:00       521  PM2.5  17.45  2021-01-24  20:30:00
     2902 2021-01-24 20:30:00+00:00       521   PM10  22.05  2021-01-24  20:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2906 2021-01-24 20:40:00+00:00       521    PM1  17.06  2021-01-24  20:40:00
     2907 2021-01-24 20:40:00+00:00       521  PM2.5  18.77  2021-01-24  20:40:00
     2908 2021-01-24 20:40:00+00:00       521   PM10  23.28  2021-01-24  20:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2912 2021-01-24 20:50:00+00:00       521    PM1  17.98  2021-01-24  20:50:00
     2913 2021-01-24 20:50:00+00:00       521  PM2.5  19.70  2021-01-24  20:50:00
     2914 2021-01-24 20:50:00+00:00       521   PM10  24.46  2021-01-24  20:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2918 2021-01-24 21:00:00+00:00       521    PM1  21.83  2021-01-24  21:00:00
     2919 2021-01-24 21:00:00+00:00       521  PM2.5  23.70  2021-01-24  21:00:00
     2920 2021-01-24 21:00:00+00:00       521   PM10  29.95  2021-01-24  21:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2924 2021-01-24 21:10:00+00:00       521    PM1  23.41  2021-01-24  21:10:00
     2925 2021-01-24 21:10:00+00:00       521  PM2.5  25.59  2021-01-24  21:10:00
     2926 2021-01-24 21:10:00+00:00       521   PM10  30.26  2021-01-24  21:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2930 2021-01-24 21:20:00+00:00       521    PM1  20.97  2021-01-24  21:20:00
     2931 2021-01-24 21:20:00+00:00       521  PM2.5  23.67  2021-01-24  21:20:00
     2932 2021-01-24 21:20:00+00:00       521   PM10  31.45  2021-01-24  21:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2936 2021-01-24 21:30:01+00:00       521    PM1  20.67  2021-01-24  21:30:01
     2937 2021-01-24 21:30:01+00:00       521  PM2.5  22.92  2021-01-24  21:30:01
     2938 2021-01-24 21:30:01+00:00       521   PM10  28.05  2021-01-24  21:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     2942 2021-01-24 21:40:00+00:00       521    PM1  18.93  2021-01-24  21:40:00
     2943 2021-01-24 21:40:00+00:00       521  PM2.5  20.69  2021-01-24  21:40:00
     2944 2021-01-24 21:40:00+00:00       521   PM10  27.01  2021-01-24  21:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2948 2021-01-24 21:50:00+00:00       521    PM1  18.98  2021-01-24  21:50:00
     2949 2021-01-24 21:50:00+00:00       521  PM2.5  21.18  2021-01-24  21:50:00
     2950 2021-01-24 21:50:00+00:00       521   PM10  27.00  2021-01-24  21:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2954 2021-01-24 22:00:00+00:00       521    PM1  18.62  2021-01-24  22:00:00
     2955 2021-01-24 22:00:00+00:00       521  PM2.5  20.84  2021-01-24  22:00:00
     2956 2021-01-24 22:00:00+00:00       521   PM10  25.25  2021-01-24  22:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     2960 2021-01-24 22:10:00+00:00       521    PM1  19.51  2021-01-24  22:10:00
     2961 2021-01-24 22:10:00+00:00       521  PM2.5  21.07  2021-01-24  22:10:00
     2962 2021-01-24 22:10:00+00:00       521   PM10  25.90  2021-01-24  22:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     2966 2021-01-24 22:20:00+00:00       521    PM1  18.63  2021-01-24  22:20:00
     2967 2021-01-24 22:20:00+00:00       521  PM2.5  20.59  2021-01-24  22:20:00
     2968 2021-01-24 22:20:00+00:00       521   PM10  24.12  2021-01-24  22:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     2972 2021-01-24 22:30:00+00:00       521    PM1  19.73  2021-01-24  22:30:00
     2973 2021-01-24 22:30:00+00:00       521  PM2.5  21.24  2021-01-24  22:30:00
     2974 2021-01-24 22:30:00+00:00       521   PM10  27.22  2021-01-24  22:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     2978 2021-01-24 22:40:00+00:00       521    PM1  17.78  2021-01-24  22:40:00
     2979 2021-01-24 22:40:00+00:00       521  PM2.5  19.32  2021-01-24  22:40:00
     2980 2021-01-24 22:40:00+00:00       521   PM10  24.84  2021-01-24  22:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     2984 2021-01-24 22:50:00+00:00       521    PM1  18.54  2021-01-24  22:50:00
     2985 2021-01-24 22:50:00+00:00       521  PM2.5  19.92  2021-01-24  22:50:00
     2986 2021-01-24 22:50:00+00:00       521   PM10  25.18  2021-01-24  22:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     2990 2021-01-24 23:00:01+00:00       521    PM1  19.00  2021-01-24  23:00:01
     2991 2021-01-24 23:00:01+00:00       521  PM2.5  20.44  2021-01-24  23:00:01
     2992 2021-01-24 23:00:01+00:00       521   PM10  23.15  2021-01-24  23:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     2996 2021-01-24 23:10:00+00:00       521    PM1  18.54  2021-01-24  23:10:00
     2997 2021-01-24 23:10:00+00:00       521  PM2.5  20.08  2021-01-24  23:10:00
     2998 2021-01-24 23:10:00+00:00       521   PM10  23.80  2021-01-24  23:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3002 2021-01-24 23:20:00+00:00       521    PM1  18.09  2021-01-24  23:20:00
     3003 2021-01-24 23:20:00+00:00       521  PM2.5  19.58  2021-01-24  23:20:00
     3004 2021-01-24 23:20:00+00:00       521   PM10  23.78  2021-01-24  23:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3008 2021-01-24 23:30:00+00:00       521    PM1  18.73  2021-01-24  23:30:00
     3009 2021-01-24 23:30:00+00:00       521  PM2.5  20.89  2021-01-24  23:30:00
     3010 2021-01-24 23:30:00+00:00       521   PM10  25.60  2021-01-24  23:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3014 2021-01-24 23:40:00+00:00       521    PM1  19.47  2021-01-24  23:40:00
     3015 2021-01-24 23:40:00+00:00       521  PM2.5  21.31  2021-01-24  23:40:00
     3016 2021-01-24 23:40:00+00:00       521   PM10  25.76  2021-01-24  23:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3020 2021-01-24 23:50:00+00:00       521    PM1  20.55  2021-01-24  23:50:00
     3021 2021-01-24 23:50:00+00:00       521  PM2.5  23.16  2021-01-24  23:50:00
     3022 2021-01-24 23:50:00+00:00       521   PM10  27.68  2021-01-24  23:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3026 2021-01-25 00:00:00+00:00       521    PM1  19.51  2021-01-25  00:00:00
     3027 2021-01-25 00:00:00+00:00       521  PM2.5  21.14  2021-01-25  00:00:00
     3028 2021-01-25 00:00:00+00:00       521   PM10  26.75  2021-01-25  00:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3032 2021-01-25 00:10:00+00:00       521    PM1  19.65  2021-01-25  00:10:00
     3033 2021-01-25 00:10:00+00:00       521  PM2.5  21.82  2021-01-25  00:10:00
     3034 2021-01-25 00:10:00+00:00       521   PM10  26.27  2021-01-25  00:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3038 2021-01-25 00:20:00+00:00       521    PM1  20.83  2021-01-25  00:20:00
     3039 2021-01-25 00:20:00+00:00       521  PM2.5  22.76  2021-01-25  00:20:00
     3040 2021-01-25 00:20:00+00:00       521   PM10  25.71  2021-01-25  00:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3044 2021-01-25 00:30:00+00:00       521    PM1  19.70  2021-01-25  00:30:00
     3045 2021-01-25 00:30:00+00:00       521  PM2.5  21.39  2021-01-25  00:30:00
     3046 2021-01-25 00:30:00+00:00       521   PM10  24.35  2021-01-25  00:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3050 2021-01-25 00:40:00+00:00       521    PM1  21.58  2021-01-25  00:40:00
     3051 2021-01-25 00:40:00+00:00       521  PM2.5  22.63  2021-01-25  00:40:00
     3052 2021-01-25 00:40:00+00:00       521   PM10  28.52  2021-01-25  00:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3056 2021-01-25 00:50:00+00:00       521    PM1  19.42  2021-01-25  00:50:00
     3057 2021-01-25 00:50:00+00:00       521  PM2.5  20.81  2021-01-25  00:50:00
     3058 2021-01-25 00:50:00+00:00       521   PM10  22.67  2021-01-25  00:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3062 2021-01-25 01:00:01+00:00       521    PM1  19.39  2021-01-25  01:00:01
     3063 2021-01-25 01:00:01+00:00       521  PM2.5  20.90  2021-01-25  01:00:01
     3064 2021-01-25 01:00:01+00:00       521   PM10  26.32  2021-01-25  01:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     3068 2021-01-25 01:10:00+00:00       521    PM1  18.93  2021-01-25  01:10:00
     3069 2021-01-25 01:10:00+00:00       521  PM2.5  20.67  2021-01-25  01:10:00
     3070 2021-01-25 01:10:00+00:00       521   PM10  26.24  2021-01-25  01:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3074 2021-01-25 01:20:00+00:00       521    PM1  20.51  2021-01-25  01:20:00
     3075 2021-01-25 01:20:00+00:00       521  PM2.5  21.85  2021-01-25  01:20:00
     3076 2021-01-25 01:20:00+00:00       521   PM10  23.85  2021-01-25  01:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3080 2021-01-25 01:30:00+00:00       521    PM1  22.43  2021-01-25  01:30:00
     3081 2021-01-25 01:30:00+00:00       521  PM2.5  24.54  2021-01-25  01:30:00
     3082 2021-01-25 01:30:00+00:00       521   PM10  29.04  2021-01-25  01:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3086 2021-01-25 01:40:00+00:00       521    PM1  21.35  2021-01-25  01:40:00
     3087 2021-01-25 01:40:00+00:00       521  PM2.5  23.03  2021-01-25  01:40:00
     3088 2021-01-25 01:40:00+00:00       521   PM10  28.19  2021-01-25  01:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3092 2021-01-25 01:50:00+00:00       521    PM1  21.21  2021-01-25  01:50:00
     3093 2021-01-25 01:50:00+00:00       521  PM2.5  22.93  2021-01-25  01:50:00
     3094 2021-01-25 01:50:00+00:00       521   PM10  27.98  2021-01-25  01:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3098 2021-01-25 02:00:00+00:00       521    PM1  21.59  2021-01-25  02:00:00
     3099 2021-01-25 02:00:00+00:00       521  PM2.5  22.97  2021-01-25  02:00:00
     3100 2021-01-25 02:00:00+00:00       521   PM10  26.46  2021-01-25  02:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3104 2021-01-25 02:10:00+00:00       521    PM1  21.99  2021-01-25  02:10:00
     3105 2021-01-25 02:10:00+00:00       521  PM2.5  23.48  2021-01-25  02:10:00
     3106 2021-01-25 02:10:00+00:00       521   PM10  28.07  2021-01-25  02:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3110 2021-01-25 02:20:00+00:00       521    PM1  21.94  2021-01-25  02:20:00
     3111 2021-01-25 02:20:00+00:00       521  PM2.5  24.08  2021-01-25  02:20:00
     3112 2021-01-25 02:20:00+00:00       521   PM10  29.07  2021-01-25  02:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3116 2021-01-25 02:30:00+00:00       521    PM1  21.11  2021-01-25  02:30:00
     3117 2021-01-25 02:30:00+00:00       521  PM2.5  23.57  2021-01-25  02:30:00
     3118 2021-01-25 02:30:00+00:00       521   PM10  29.68  2021-01-25  02:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3122 2021-01-25 02:40:00+00:00       521    PM1  22.54  2021-01-25  02:40:00
     3123 2021-01-25 02:40:00+00:00       521  PM2.5  24.80  2021-01-25  02:40:00
     3124 2021-01-25 02:40:00+00:00       521   PM10  31.70  2021-01-25  02:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3128 2021-01-25 02:50:00+00:00       521    PM1  20.76  2021-01-25  02:50:00
     3129 2021-01-25 02:50:00+00:00       521  PM2.5  22.45  2021-01-25  02:50:00
     3130 2021-01-25 02:50:00+00:00       521   PM10  23.98  2021-01-25  02:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3134 2021-01-25 03:00:00+00:00       521    PM1  21.66  2021-01-25  03:00:00
     3135 2021-01-25 03:00:00+00:00       521  PM2.5  23.18  2021-01-25  03:00:00
     3136 2021-01-25 03:00:00+00:00       521   PM10  26.01  2021-01-25  03:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3140 2021-01-25 03:10:01+00:00       521    PM1  22.30  2021-01-25  03:10:01
     3141 2021-01-25 03:10:01+00:00       521  PM2.5  23.61  2021-01-25  03:10:01
     3142 2021-01-25 03:10:01+00:00       521   PM10  29.71  2021-01-25  03:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     3146 2021-01-25 03:20:00+00:00       521    PM1  21.40  2021-01-25  03:20:00
     3147 2021-01-25 03:20:00+00:00       521  PM2.5  23.06  2021-01-25  03:20:00
     3148 2021-01-25 03:20:00+00:00       521   PM10  28.36  2021-01-25  03:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3152 2021-01-25 03:30:00+00:00       521    PM1  21.20  2021-01-25  03:30:00
     3153 2021-01-25 03:30:00+00:00       521  PM2.5  23.47  2021-01-25  03:30:00
     3154 2021-01-25 03:30:00+00:00       521   PM10  24.83  2021-01-25  03:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3161 2021-01-25 03:50:00+00:00       521    PM1  22.41  2021-01-25  03:50:00
     3162 2021-01-25 03:50:00+00:00       521  PM2.5  23.73  2021-01-25  03:50:00
     3163 2021-01-25 03:50:00+00:00       521   PM10  28.74  2021-01-25  03:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3167 2021-01-25 04:00:00+00:00       521    PM1  19.99  2021-01-25  04:00:00
     3168 2021-01-25 04:00:00+00:00       521  PM2.5  21.79  2021-01-25  04:00:00
     3169 2021-01-25 04:00:00+00:00       521   PM10  27.91  2021-01-25  04:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3173 2021-01-25 04:10:00+00:00       521    PM1  20.80  2021-01-25  04:10:00
     3174 2021-01-25 04:10:00+00:00       521  PM2.5  22.33  2021-01-25  04:10:00
     3175 2021-01-25 04:10:00+00:00       521   PM10  27.40  2021-01-25  04:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3179 2021-01-25 04:20:01+00:00       521    PM1  24.56  2021-01-25  04:20:01
     3180 2021-01-25 04:20:01+00:00       521  PM2.5  27.63  2021-01-25  04:20:01
     3181 2021-01-25 04:20:01+00:00       521   PM10  30.47  2021-01-25  04:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     3185 2021-01-25 04:30:00+00:00       521    PM1  24.91  2021-01-25  04:30:00
     3186 2021-01-25 04:30:00+00:00       521  PM2.5  27.62  2021-01-25  04:30:00
     3187 2021-01-25 04:30:00+00:00       521   PM10  31.93  2021-01-25  04:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3191 2021-01-25 04:40:00+00:00       521    PM1  24.98  2021-01-25  04:40:00
     3192 2021-01-25 04:40:00+00:00       521  PM2.5  27.84  2021-01-25  04:40:00
     3193 2021-01-25 04:40:00+00:00       521   PM10  32.78  2021-01-25  04:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3197 2021-01-25 04:50:00+00:00       521    PM1  25.33  2021-01-25  04:50:00
     3198 2021-01-25 04:50:00+00:00       521  PM2.5  28.06  2021-01-25  04:50:00
     3199 2021-01-25 04:50:00+00:00       521   PM10  31.27  2021-01-25  04:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3203 2021-01-25 05:00:00+00:00       521    PM1  23.19  2021-01-25  05:00:00
     3204 2021-01-25 05:00:00+00:00       521  PM2.5  25.27  2021-01-25  05:00:00
     3205 2021-01-25 05:00:00+00:00       521   PM10  29.37  2021-01-25  05:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3209 2021-01-25 05:10:00+00:00       521    PM1  23.23  2021-01-25  05:10:00
     3210 2021-01-25 05:10:00+00:00       521  PM2.5  24.66  2021-01-25  05:10:00
     3211 2021-01-25 05:10:00+00:00       521   PM10  30.69  2021-01-25  05:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3215 2021-01-25 05:20:00+00:00       521    PM1  23.88  2021-01-25  05:20:00
     3216 2021-01-25 05:20:00+00:00       521  PM2.5  26.19  2021-01-25  05:20:00
     3217 2021-01-25 05:20:00+00:00       521   PM10  29.59  2021-01-25  05:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3221 2021-01-25 05:30:00+00:00       521    PM1  25.00  2021-01-25  05:30:00
     3222 2021-01-25 05:30:00+00:00       521  PM2.5  27.20  2021-01-25  05:30:00
     3223 2021-01-25 05:30:00+00:00       521   PM10  31.53  2021-01-25  05:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3227 2021-01-25 05:40:00+00:00       521    PM1  23.88  2021-01-25  05:40:00
     3228 2021-01-25 05:40:00+00:00       521  PM2.5  25.73  2021-01-25  05:40:00
     3229 2021-01-25 05:40:00+00:00       521   PM10  31.83  2021-01-25  05:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3233 2021-01-25 05:50:00+00:00       521    PM1  24.83  2021-01-25  05:50:00
     3234 2021-01-25 05:50:00+00:00       521  PM2.5  26.26  2021-01-25  05:50:00
     3235 2021-01-25 05:50:00+00:00       521   PM10  32.08  2021-01-25  05:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3239 2021-01-25 06:00:00+00:00       521    PM1  23.73  2021-01-25  06:00:00
     3240 2021-01-25 06:00:00+00:00       521  PM2.5  25.74  2021-01-25  06:00:00
     3241 2021-01-25 06:00:00+00:00       521   PM10  28.45  2021-01-25  06:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3245 2021-01-25 06:10:00+00:00       521    PM1  24.03  2021-01-25  06:10:00
     3246 2021-01-25 06:10:00+00:00       521  PM2.5  26.26  2021-01-25  06:10:00
     3247 2021-01-25 06:10:00+00:00       521   PM10  29.40  2021-01-25  06:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3251 2021-01-25 06:20:00+00:00       521    PM1  24.19  2021-01-25  06:20:00
     3252 2021-01-25 06:20:00+00:00       521  PM2.5  26.08  2021-01-25  06:20:00
     3253 2021-01-25 06:20:00+00:00       521   PM10  31.09  2021-01-25  06:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3257 2021-01-25 06:30:01+00:00       521    PM1  22.91  2021-01-25  06:30:01
     3258 2021-01-25 06:30:01+00:00       521  PM2.5  24.47  2021-01-25  06:30:01
     3259 2021-01-25 06:30:01+00:00       521   PM10  28.99  2021-01-25  06:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     3263 2021-01-25 06:39:59+00:00       521    PM1  24.12  2021-01-25  06:39:59
     3264 2021-01-25 06:39:59+00:00       521  PM2.5  25.60  2021-01-25  06:39:59
     3265 2021-01-25 06:39:59+00:00       521   PM10  31.08  2021-01-25  06:39:59,
                          Timestamp  Location   Name  Value        Date      Time
     3269 2021-01-25 06:50:00+00:00       521    PM1  24.65  2021-01-25  06:50:00
     3270 2021-01-25 06:50:00+00:00       521  PM2.5  26.19  2021-01-25  06:50:00
     3271 2021-01-25 06:50:00+00:00       521   PM10  29.07  2021-01-25  06:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3275 2021-01-25 07:00:01+00:00       521    PM1  25.11  2021-01-25  07:00:01
     3276 2021-01-25 07:00:01+00:00       521  PM2.5  26.75  2021-01-25  07:00:01
     3277 2021-01-25 07:00:01+00:00       521   PM10  34.20  2021-01-25  07:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     3281 2021-01-25 07:10:00+00:00       521    PM1  23.79  2021-01-25  07:10:00
     3282 2021-01-25 07:10:00+00:00       521  PM2.5  25.50  2021-01-25  07:10:00
     3283 2021-01-25 07:10:00+00:00       521   PM10  30.89  2021-01-25  07:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3287 2021-01-25 07:20:00+00:00       521    PM1  24.83  2021-01-25  07:20:00
     3288 2021-01-25 07:20:00+00:00       521  PM2.5  26.68  2021-01-25  07:20:00
     3289 2021-01-25 07:20:00+00:00       521   PM10  31.27  2021-01-25  07:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3293 2021-01-25 07:30:00+00:00       521    PM1  23.85  2021-01-25  07:30:00
     3294 2021-01-25 07:30:00+00:00       521  PM2.5  25.19  2021-01-25  07:30:00
     3295 2021-01-25 07:30:00+00:00       521   PM10  25.16  2021-01-25  07:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3299 2021-01-25 07:40:00+00:00       521    PM1  23.00  2021-01-25  07:40:00
     3300 2021-01-25 07:40:00+00:00       521  PM2.5  24.97  2021-01-25  07:40:00
     3301 2021-01-25 07:40:00+00:00       521   PM10  29.31  2021-01-25  07:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3305 2021-01-25 07:50:00+00:00       521    PM1  24.55  2021-01-25  07:50:00
     3306 2021-01-25 07:50:00+00:00       521  PM2.5  26.42  2021-01-25  07:50:00
     3307 2021-01-25 07:50:00+00:00       521   PM10  28.25  2021-01-25  07:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3311 2021-01-25 08:00:00+00:00       521    PM1  22.33  2021-01-25  08:00:00
     3312 2021-01-25 08:00:00+00:00       521  PM2.5  23.91  2021-01-25  08:00:00
     3313 2021-01-25 08:00:00+00:00       521   PM10  28.80  2021-01-25  08:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3317 2021-01-25 08:10:00+00:00       521    PM1  24.74  2021-01-25  08:10:00
     3318 2021-01-25 08:10:00+00:00       521  PM2.5  26.74  2021-01-25  08:10:00
     3319 2021-01-25 08:10:00+00:00       521   PM10  27.85  2021-01-25  08:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3323 2021-01-25 08:20:01+00:00       521    PM1  23.90  2021-01-25  08:20:01
     3324 2021-01-25 08:20:01+00:00       521  PM2.5  25.72  2021-01-25  08:20:01
     3325 2021-01-25 08:20:01+00:00       521   PM10  31.34  2021-01-25  08:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     3329 2021-01-25 08:30:00+00:00       521    PM1  24.69  2021-01-25  08:30:00
     3330 2021-01-25 08:30:00+00:00       521  PM2.5  27.23  2021-01-25  08:30:00
     3331 2021-01-25 08:30:00+00:00       521   PM10  34.03  2021-01-25  08:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3335 2021-01-25 08:40:00+00:00       521    PM1  24.84  2021-01-25  08:40:00
     3336 2021-01-25 08:40:00+00:00       521  PM2.5  28.31  2021-01-25  08:40:00
     3337 2021-01-25 08:40:00+00:00       521   PM10  36.82  2021-01-25  08:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3341 2021-01-25 08:50:00+00:00       521    PM1  23.72  2021-01-25  08:50:00
     3342 2021-01-25 08:50:00+00:00       521  PM2.5  26.10  2021-01-25  08:50:00
     3343 2021-01-25 08:50:00+00:00       521   PM10  34.12  2021-01-25  08:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3347 2021-01-25 09:00:00+00:00       521    PM1  25.22  2021-01-25  09:00:00
     3348 2021-01-25 09:00:00+00:00       521  PM2.5  30.11  2021-01-25  09:00:00
     3349 2021-01-25 09:00:00+00:00       521   PM10  37.25  2021-01-25  09:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3353 2021-01-25 09:10:00+00:00       521    PM1  23.84  2021-01-25  09:10:00
     3354 2021-01-25 09:10:00+00:00       521  PM2.5  26.54  2021-01-25  09:10:00
     3355 2021-01-25 09:10:00+00:00       521   PM10  31.37  2021-01-25  09:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3359 2021-01-25 09:20:00+00:00       521    PM1  22.97  2021-01-25  09:20:00
     3360 2021-01-25 09:20:00+00:00       521  PM2.5  25.55  2021-01-25  09:20:00
     3361 2021-01-25 09:20:00+00:00       521   PM10  32.51  2021-01-25  09:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3365 2021-01-25 09:30:00+00:00       521    PM1  23.03  2021-01-25  09:30:00
     3366 2021-01-25 09:30:00+00:00       521  PM2.5  25.01  2021-01-25  09:30:00
     3367 2021-01-25 09:30:00+00:00       521   PM10  31.53  2021-01-25  09:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3371 2021-01-25 09:40:00+00:00       521    PM1  21.13  2021-01-25  09:40:00
     3372 2021-01-25 09:40:00+00:00       521  PM2.5  22.90  2021-01-25  09:40:00
     3373 2021-01-25 09:40:00+00:00       521   PM10  30.20  2021-01-25  09:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3377 2021-01-25 09:50:00+00:00       521    PM1  20.79  2021-01-25  09:50:00
     3378 2021-01-25 09:50:00+00:00       521  PM2.5  22.32  2021-01-25  09:50:00
     3379 2021-01-25 09:50:00+00:00       521   PM10  29.56  2021-01-25  09:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3383 2021-01-25 10:00:00+00:00       521    PM1  19.49  2021-01-25  10:00:00
     3384 2021-01-25 10:00:00+00:00       521  PM2.5  21.15  2021-01-25  10:00:00
     3385 2021-01-25 10:00:00+00:00       521   PM10  28.02  2021-01-25  10:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3389 2021-01-25 10:10:00+00:00       521    PM1  18.63  2021-01-25  10:10:00
     3390 2021-01-25 10:10:00+00:00       521  PM2.5  20.44  2021-01-25  10:10:00
     3391 2021-01-25 10:10:00+00:00       521   PM10  25.11  2021-01-25  10:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3395 2021-01-25 10:20:00+00:00       521    PM1  16.03  2021-01-25  10:20:00
     3396 2021-01-25 10:20:00+00:00       521  PM2.5  17.52  2021-01-25  10:20:00
     3397 2021-01-25 10:20:00+00:00       521   PM10  21.81  2021-01-25  10:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3401 2021-01-25 10:30:01+00:00       521    PM1  12.02  2021-01-25  10:30:01
     3402 2021-01-25 10:30:01+00:00       521  PM2.5  12.92  2021-01-25  10:30:01
     3403 2021-01-25 10:30:01+00:00       521   PM10  16.22  2021-01-25  10:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     3407 2021-01-25 10:40:00+00:00       521    PM1  16.92  2021-01-25  10:40:00
     3408 2021-01-25 10:40:00+00:00       521  PM2.5  18.43  2021-01-25  10:40:00
     3409 2021-01-25 10:40:00+00:00       521   PM10  23.64  2021-01-25  10:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3413 2021-01-25 10:50:00+00:00       521    PM1   8.79  2021-01-25  10:50:00
     3414 2021-01-25 10:50:00+00:00       521  PM2.5  10.50  2021-01-25  10:50:00
     3415 2021-01-25 10:50:00+00:00       521   PM10  14.60  2021-01-25  10:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3419 2021-01-25 11:00:00+00:00       521    PM1   5.32  2021-01-25  11:00:00
     3420 2021-01-25 11:00:00+00:00       521  PM2.5   6.05  2021-01-25  11:00:00
     3421 2021-01-25 11:00:00+00:00       521   PM10   9.56  2021-01-25  11:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3425 2021-01-25 11:10:00+00:00       521    PM1   4.65  2021-01-25  11:10:00
     3426 2021-01-25 11:10:00+00:00       521  PM2.5   5.38  2021-01-25  11:10:00
     3427 2021-01-25 11:10:00+00:00       521   PM10   8.32  2021-01-25  11:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3431 2021-01-25 11:20:00+00:00       521    PM1   4.06  2021-01-25  11:20:00
     3432 2021-01-25 11:20:00+00:00       521  PM2.5   4.73  2021-01-25  11:20:00
     3433 2021-01-25 11:20:00+00:00       521   PM10   8.03  2021-01-25  11:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3437 2021-01-25 11:30:00+00:00       521    PM1   3.29  2021-01-25  11:30:00
     3438 2021-01-25 11:30:00+00:00       521  PM2.5   4.38  2021-01-25  11:30:00
     3439 2021-01-25 11:30:00+00:00       521   PM10   7.50  2021-01-25  11:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3443 2021-01-25 11:40:00+00:00       521    PM1   3.94  2021-01-25  11:40:00
     3444 2021-01-25 11:40:00+00:00       521  PM2.5   4.45  2021-01-25  11:40:00
     3445 2021-01-25 11:40:00+00:00       521   PM10   7.26  2021-01-25  11:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3449 2021-01-25 11:50:00+00:00       521    PM1   2.87  2021-01-25  11:50:00
     3450 2021-01-25 11:50:00+00:00       521  PM2.5   3.83  2021-01-25  11:50:00
     3451 2021-01-25 11:50:00+00:00       521   PM10   6.35  2021-01-25  11:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3455 2021-01-25 12:00:00+00:00       521    PM1   2.74  2021-01-25  12:00:00
     3456 2021-01-25 12:00:00+00:00       521  PM2.5   3.42  2021-01-25  12:00:00
     3457 2021-01-25 12:00:00+00:00       521   PM10  11.65  2021-01-25  12:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3461 2021-01-25 12:10:01+00:00       521    PM1   2.63  2021-01-25  12:10:01
     3462 2021-01-25 12:10:01+00:00       521  PM2.5   3.14  2021-01-25  12:10:01
     3463 2021-01-25 12:10:01+00:00       521   PM10   9.38  2021-01-25  12:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     3467 2021-01-25 12:19:59+00:00       521    PM1   3.13  2021-01-25  12:19:59
     3468 2021-01-25 12:19:59+00:00       521  PM2.5   3.89  2021-01-25  12:19:59
     3469 2021-01-25 12:19:59+00:00       521   PM10   9.12  2021-01-25  12:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     3473 2021-01-25 12:30:01+00:00       521    PM1   2.66  2021-01-25  12:30:01
     3474 2021-01-25 12:30:01+00:00       521  PM2.5   4.00  2021-01-25  12:30:01
     3475 2021-01-25 12:30:01+00:00       521   PM10   6.42  2021-01-25  12:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     3479 2021-01-25 12:40:00+00:00       521    PM1   2.48  2021-01-25  12:40:00
     3480 2021-01-25 12:40:00+00:00       521  PM2.5   3.17  2021-01-25  12:40:00
     3481 2021-01-25 12:40:00+00:00       521   PM10   8.86  2021-01-25  12:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3485 2021-01-25 12:50:00+00:00       521    PM1   3.91  2021-01-25  12:50:00
     3486 2021-01-25 12:50:00+00:00       521  PM2.5   4.91  2021-01-25  12:50:00
     3487 2021-01-25 12:50:00+00:00       521   PM10  10.39  2021-01-25  12:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3491 2021-01-25 13:00:00+00:00       521    PM1   3.56  2021-01-25  13:00:00
     3492 2021-01-25 13:00:00+00:00       521  PM2.5   4.45  2021-01-25  13:00:00
     3493 2021-01-25 13:00:00+00:00       521   PM10  11.73  2021-01-25  13:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3497 2021-01-25 13:10:00+00:00       521    PM1   3.76  2021-01-25  13:10:00
     3498 2021-01-25 13:10:00+00:00       521  PM2.5   4.77  2021-01-25  13:10:00
     3499 2021-01-25 13:10:00+00:00       521   PM10   7.68  2021-01-25  13:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3503 2021-01-25 13:20:00+00:00       521    PM1   3.45  2021-01-25  13:20:00
     3504 2021-01-25 13:20:00+00:00       521  PM2.5   4.38  2021-01-25  13:20:00
     3505 2021-01-25 13:20:00+00:00       521   PM10   8.04  2021-01-25  13:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3509 2021-01-25 13:30:00+00:00       521    PM1   3.92  2021-01-25  13:30:00
     3510 2021-01-25 13:30:00+00:00       521  PM2.5   4.91  2021-01-25  13:30:00
     3511 2021-01-25 13:30:00+00:00       521   PM10   9.38  2021-01-25  13:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3515 2021-01-25 13:40:01+00:00       521    PM1   4.84  2021-01-25  13:40:01
     3516 2021-01-25 13:40:01+00:00       521  PM2.5   6.39  2021-01-25  13:40:01
     3517 2021-01-25 13:40:01+00:00       521   PM10  10.92  2021-01-25  13:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     3521 2021-01-25 13:50:00+00:00       521    PM1   3.88  2021-01-25  13:50:00
     3522 2021-01-25 13:50:00+00:00       521  PM2.5   4.82  2021-01-25  13:50:00
     3523 2021-01-25 13:50:00+00:00       521   PM10  14.17  2021-01-25  13:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3527 2021-01-25 14:00:00+00:00       521    PM1   4.18  2021-01-25  14:00:00
     3528 2021-01-25 14:00:00+00:00       521  PM2.5   5.94  2021-01-25  14:00:00
     3529 2021-01-25 14:00:00+00:00       521   PM10  12.99  2021-01-25  14:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3533 2021-01-25 14:10:00+00:00       521    PM1   4.23  2021-01-25  14:10:00
     3534 2021-01-25 14:10:00+00:00       521  PM2.5   5.71  2021-01-25  14:10:00
     3535 2021-01-25 14:10:00+00:00       521   PM10   9.83  2021-01-25  14:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3539 2021-01-25 14:20:00+00:00       521    PM1   4.81  2021-01-25  14:20:00
     3540 2021-01-25 14:20:00+00:00       521  PM2.5   6.54  2021-01-25  14:20:00
     3541 2021-01-25 14:20:00+00:00       521   PM10  15.11  2021-01-25  14:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3545 2021-01-25 14:30:00+00:00       521    PM1   3.98  2021-01-25  14:30:00
     3546 2021-01-25 14:30:00+00:00       521  PM2.5   5.59  2021-01-25  14:30:00
     3547 2021-01-25 14:30:00+00:00       521   PM10   9.56  2021-01-25  14:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3551 2021-01-25 14:40:00+00:00       521    PM1   4.52  2021-01-25  14:40:00
     3552 2021-01-25 14:40:00+00:00       521  PM2.5   6.08  2021-01-25  14:40:00
     3553 2021-01-25 14:40:00+00:00       521   PM10  12.31  2021-01-25  14:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3557 2021-01-25 14:50:00+00:00       521    PM1   3.80  2021-01-25  14:50:00
     3558 2021-01-25 14:50:00+00:00       521  PM2.5   5.13  2021-01-25  14:50:00
     3559 2021-01-25 14:50:00+00:00       521   PM10  11.10  2021-01-25  14:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3563 2021-01-25 15:00:00+00:00       521    PM1   3.37  2021-01-25  15:00:00
     3564 2021-01-25 15:00:00+00:00       521  PM2.5   4.74  2021-01-25  15:00:00
     3565 2021-01-25 15:00:00+00:00       521   PM10  11.05  2021-01-25  15:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3569 2021-01-25 15:10:00+00:00       521    PM1   3.66  2021-01-25  15:10:00
     3570 2021-01-25 15:10:00+00:00       521  PM2.5   4.82  2021-01-25  15:10:00
     3571 2021-01-25 15:10:00+00:00       521   PM10  11.94  2021-01-25  15:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3575 2021-01-25 15:20:00+00:00       521    PM1   5.61  2021-01-25  15:20:00
     3576 2021-01-25 15:20:00+00:00       521  PM2.5   7.29  2021-01-25  15:20:00
     3577 2021-01-25 15:20:00+00:00       521   PM10  16.92  2021-01-25  15:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3581 2021-01-25 15:30:00+00:00       521    PM1   6.02  2021-01-25  15:30:00
     3582 2021-01-25 15:30:00+00:00       521  PM2.5   7.84  2021-01-25  15:30:00
     3583 2021-01-25 15:30:00+00:00       521   PM10  12.14  2021-01-25  15:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3587 2021-01-25 15:40:00+00:00       521    PM1   6.88  2021-01-25  15:40:00
     3588 2021-01-25 15:40:00+00:00       521  PM2.5   9.81  2021-01-25  15:40:00
     3589 2021-01-25 15:40:00+00:00       521   PM10  17.67  2021-01-25  15:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3593 2021-01-25 15:50:00+00:00       521    PM1   7.77  2021-01-25  15:50:00
     3594 2021-01-25 15:50:00+00:00       521  PM2.5  11.27  2021-01-25  15:50:00
     3595 2021-01-25 15:50:00+00:00       521   PM10  20.46  2021-01-25  15:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3599 2021-01-25 16:00:00+00:00       521    PM1   6.78  2021-01-25  16:00:00
     3600 2021-01-25 16:00:00+00:00       521  PM2.5   9.71  2021-01-25  16:00:00
     3601 2021-01-25 16:00:00+00:00       521   PM10  19.37  2021-01-25  16:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3605 2021-01-25 16:10:00+00:00       521    PM1   6.56  2021-01-25  16:10:00
     3606 2021-01-25 16:10:00+00:00       521  PM2.5   8.62  2021-01-25  16:10:00
     3607 2021-01-25 16:10:00+00:00       521   PM10  20.17  2021-01-25  16:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3611 2021-01-25 16:20:01+00:00       521    PM1   7.67  2021-01-25  16:20:01
     3612 2021-01-25 16:20:01+00:00       521  PM2.5  10.34  2021-01-25  16:20:01
     3613 2021-01-25 16:20:01+00:00       521   PM10  19.60  2021-01-25  16:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     3617 2021-01-25 16:30:00+00:00       521    PM1   6.88  2021-01-25  16:30:00
     3618 2021-01-25 16:30:00+00:00       521  PM2.5   9.62  2021-01-25  16:30:00
     3619 2021-01-25 16:30:00+00:00       521   PM10  16.45  2021-01-25  16:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3623 2021-01-25 16:40:00+00:00       521    PM1   6.13  2021-01-25  16:40:00
     3624 2021-01-25 16:40:00+00:00       521  PM2.5   8.46  2021-01-25  16:40:00
     3625 2021-01-25 16:40:00+00:00       521   PM10  17.77  2021-01-25  16:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3629 2021-01-25 16:50:00+00:00       521    PM1   6.35  2021-01-25  16:50:00
     3630 2021-01-25 16:50:00+00:00       521  PM2.5   9.20  2021-01-25  16:50:00
     3631 2021-01-25 16:50:00+00:00       521   PM10  18.83  2021-01-25  16:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3635 2021-01-25 17:00:00+00:00       521    PM1   6.39  2021-01-25  17:00:00
     3636 2021-01-25 17:00:00+00:00       521  PM2.5   8.44  2021-01-25  17:00:00
     3637 2021-01-25 17:00:00+00:00       521   PM10  17.92  2021-01-25  17:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3641 2021-01-25 17:10:00+00:00       521    PM1   6.39  2021-01-25  17:10:00
     3642 2021-01-25 17:10:00+00:00       521  PM2.5   9.18  2021-01-25  17:10:00
     3643 2021-01-25 17:10:00+00:00       521   PM10  16.42  2021-01-25  17:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3647 2021-01-25 17:20:00+00:00       521    PM1   7.31  2021-01-25  17:20:00
     3648 2021-01-25 17:20:00+00:00       521  PM2.5  10.10  2021-01-25  17:20:00
     3649 2021-01-25 17:20:00+00:00       521   PM10  22.66  2021-01-25  17:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3653 2021-01-25 17:30:00+00:00       521    PM1   7.40  2021-01-25  17:30:00
     3654 2021-01-25 17:30:00+00:00       521  PM2.5  10.69  2021-01-25  17:30:00
     3655 2021-01-25 17:30:00+00:00       521   PM10  21.21  2021-01-25  17:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3659 2021-01-25 17:40:01+00:00       521    PM1   7.81  2021-01-25  17:40:01
     3660 2021-01-25 17:40:01+00:00       521  PM2.5  11.89  2021-01-25  17:40:01
     3661 2021-01-25 17:40:01+00:00       521   PM10  20.91  2021-01-25  17:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     3665 2021-01-25 17:50:00+00:00       521    PM1   8.04  2021-01-25  17:50:00
     3666 2021-01-25 17:50:00+00:00       521  PM2.5  11.92  2021-01-25  17:50:00
     3667 2021-01-25 17:50:00+00:00       521   PM10  23.14  2021-01-25  17:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3671 2021-01-25 18:00:00+00:00       521    PM1   7.25  2021-01-25  18:00:00
     3672 2021-01-25 18:00:00+00:00       521  PM2.5  10.58  2021-01-25  18:00:00
     3673 2021-01-25 18:00:00+00:00       521   PM10  18.54  2021-01-25  18:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3677 2021-01-25 18:10:00+00:00       521    PM1   5.25  2021-01-25  18:10:00
     3678 2021-01-25 18:10:00+00:00       521  PM2.5   8.66  2021-01-25  18:10:00
     3679 2021-01-25 18:10:00+00:00       521   PM10  13.97  2021-01-25  18:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3683 2021-01-25 18:20:00+00:00       521    PM1   5.84  2021-01-25  18:20:00
     3684 2021-01-25 18:20:00+00:00       521  PM2.5   8.16  2021-01-25  18:20:00
     3685 2021-01-25 18:20:00+00:00       521   PM10  12.49  2021-01-25  18:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3689 2021-01-25 18:30:00+00:00       521    PM1   7.57  2021-01-25  18:30:00
     3690 2021-01-25 18:30:00+00:00       521  PM2.5  11.25  2021-01-25  18:30:00
     3691 2021-01-25 18:30:00+00:00       521   PM10  22.36  2021-01-25  18:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3695 2021-01-25 18:40:00+00:00       521    PM1   7.86  2021-01-25  18:40:00
     3696 2021-01-25 18:40:00+00:00       521  PM2.5  11.84  2021-01-25  18:40:00
     3697 2021-01-25 18:40:00+00:00       521   PM10  21.47  2021-01-25  18:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3701 2021-01-25 18:50:00+00:00       521    PM1   8.38  2021-01-25  18:50:00
     3702 2021-01-25 18:50:00+00:00       521  PM2.5  13.32  2021-01-25  18:50:00
     3703 2021-01-25 18:50:00+00:00       521   PM10  25.83  2021-01-25  18:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3707 2021-01-25 19:00:00+00:00       521    PM1   7.39  2021-01-25  19:00:00
     3708 2021-01-25 19:00:00+00:00       521  PM2.5  11.61  2021-01-25  19:00:00
     3709 2021-01-25 19:00:00+00:00       521   PM10  25.07  2021-01-25  19:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3713 2021-01-25 19:10:00+00:00       521    PM1   7.21  2021-01-25  19:10:00
     3714 2021-01-25 19:10:00+00:00       521  PM2.5   9.32  2021-01-25  19:10:00
     3715 2021-01-25 19:10:00+00:00       521   PM10  17.96  2021-01-25  19:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3719 2021-01-25 19:20:00+00:00       521    PM1   8.02  2021-01-25  19:20:00
     3720 2021-01-25 19:20:00+00:00       521  PM2.5  13.02  2021-01-25  19:20:00
     3721 2021-01-25 19:20:00+00:00       521   PM10  23.27  2021-01-25  19:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3725 2021-01-25 19:30:00+00:00       521    PM1   8.34  2021-01-25  19:30:00
     3726 2021-01-25 19:30:00+00:00       521  PM2.5  13.04  2021-01-25  19:30:00
     3727 2021-01-25 19:30:00+00:00       521   PM10  25.34  2021-01-25  19:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3731 2021-01-25 19:40:00+00:00       521    PM1   4.89  2021-01-25  19:40:00
     3732 2021-01-25 19:40:00+00:00       521  PM2.5   6.52  2021-01-25  19:40:00
     3733 2021-01-25 19:40:00+00:00       521   PM10  12.57  2021-01-25  19:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3737 2021-01-25 19:50:00+00:00       521    PM1   4.99  2021-01-25  19:50:00
     3738 2021-01-25 19:50:00+00:00       521  PM2.5   6.70  2021-01-25  19:50:00
     3739 2021-01-25 19:50:00+00:00       521   PM10  12.22  2021-01-25  19:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3743 2021-01-25 20:00:01+00:00       521    PM1   6.80  2021-01-25  20:00:01
     3744 2021-01-25 20:00:01+00:00       521  PM2.5   9.20  2021-01-25  20:00:01
     3745 2021-01-25 20:00:01+00:00       521   PM10  17.33  2021-01-25  20:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     3749 2021-01-25 20:10:00+00:00       521    PM1   6.78  2021-01-25  20:10:00
     3750 2021-01-25 20:10:00+00:00       521  PM2.5   9.66  2021-01-25  20:10:00
     3751 2021-01-25 20:10:00+00:00       521   PM10  19.91  2021-01-25  20:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3755 2021-01-25 20:20:00+00:00       521    PM1   6.31  2021-01-25  20:20:00
     3756 2021-01-25 20:20:00+00:00       521  PM2.5   9.52  2021-01-25  20:20:00
     3757 2021-01-25 20:20:00+00:00       521   PM10  19.92  2021-01-25  20:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3761 2021-01-25 20:30:00+00:00       521    PM1   5.87  2021-01-25  20:30:00
     3762 2021-01-25 20:30:00+00:00       521  PM2.5   8.70  2021-01-25  20:30:00
     3763 2021-01-25 20:30:00+00:00       521   PM10  20.07  2021-01-25  20:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3767 2021-01-25 20:40:00+00:00       521    PM1   5.48  2021-01-25  20:40:00
     3768 2021-01-25 20:40:00+00:00       521  PM2.5   8.63  2021-01-25  20:40:00
     3769 2021-01-25 20:40:00+00:00       521   PM10  18.38  2021-01-25  20:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3773 2021-01-25 20:50:00+00:00       521    PM1   6.22  2021-01-25  20:50:00
     3774 2021-01-25 20:50:00+00:00       521  PM2.5   9.28  2021-01-25  20:50:00
     3775 2021-01-25 20:50:00+00:00       521   PM10  20.95  2021-01-25  20:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3779 2021-01-25 21:00:00+00:00       521    PM1   7.15  2021-01-25  21:00:00
     3780 2021-01-25 21:00:00+00:00       521  PM2.5  10.66  2021-01-25  21:00:00
     3781 2021-01-25 21:00:00+00:00       521   PM10  21.64  2021-01-25  21:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3785 2021-01-25 21:10:00+00:00       521    PM1   6.28  2021-01-25  21:10:00
     3786 2021-01-25 21:10:00+00:00       521  PM2.5  10.06  2021-01-25  21:10:00
     3787 2021-01-25 21:10:00+00:00       521   PM10  25.36  2021-01-25  21:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3791 2021-01-25 21:20:00+00:00       521    PM1   7.39  2021-01-25  21:20:00
     3792 2021-01-25 21:20:00+00:00       521  PM2.5  11.51  2021-01-25  21:20:00
     3793 2021-01-25 21:20:00+00:00       521   PM10  20.78  2021-01-25  21:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3797 2021-01-25 21:30:00+00:00       521    PM1   7.13  2021-01-25  21:30:00
     3798 2021-01-25 21:30:00+00:00       521  PM2.5  11.54  2021-01-25  21:30:00
     3799 2021-01-25 21:30:00+00:00       521   PM10  22.41  2021-01-25  21:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3803 2021-01-25 21:40:01+00:00       521    PM1   6.94  2021-01-25  21:40:01
     3804 2021-01-25 21:40:01+00:00       521  PM2.5  10.38  2021-01-25  21:40:01
     3805 2021-01-25 21:40:01+00:00       521   PM10  18.95  2021-01-25  21:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     3809 2021-01-25 21:50:00+00:00       521    PM1   6.73  2021-01-25  21:50:00
     3810 2021-01-25 21:50:00+00:00       521  PM2.5   9.22  2021-01-25  21:50:00
     3811 2021-01-25 21:50:00+00:00       521   PM10  20.26  2021-01-25  21:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3815 2021-01-25 22:00:00+00:00       521    PM1   6.04  2021-01-25  22:00:00
     3816 2021-01-25 22:00:00+00:00       521  PM2.5   9.06  2021-01-25  22:00:00
     3817 2021-01-25 22:00:00+00:00       521   PM10  17.01  2021-01-25  22:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3821 2021-01-25 22:10:00+00:00       521    PM1   8.00  2021-01-25  22:10:00
     3822 2021-01-25 22:10:00+00:00       521  PM2.5  12.04  2021-01-25  22:10:00
     3823 2021-01-25 22:10:00+00:00       521   PM10  23.70  2021-01-25  22:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3827 2021-01-25 22:20:00+00:00       521    PM1   7.59  2021-01-25  22:20:00
     3828 2021-01-25 22:20:00+00:00       521  PM2.5  10.89  2021-01-25  22:20:00
     3829 2021-01-25 22:20:00+00:00       521   PM10  17.80  2021-01-25  22:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3833 2021-01-25 22:30:00+00:00       521    PM1   7.48  2021-01-25  22:30:00
     3834 2021-01-25 22:30:00+00:00       521  PM2.5  12.13  2021-01-25  22:30:00
     3835 2021-01-25 22:30:00+00:00       521   PM10  24.13  2021-01-25  22:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3839 2021-01-25 22:40:00+00:00       521    PM1   6.80  2021-01-25  22:40:00
     3840 2021-01-25 22:40:00+00:00       521  PM2.5  10.66  2021-01-25  22:40:00
     3841 2021-01-25 22:40:00+00:00       521   PM10  22.09  2021-01-25  22:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3845 2021-01-25 22:50:00+00:00       521    PM1   7.84  2021-01-25  22:50:00
     3846 2021-01-25 22:50:00+00:00       521  PM2.5  12.37  2021-01-25  22:50:00
     3847 2021-01-25 22:50:00+00:00       521   PM10  23.00  2021-01-25  22:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3851 2021-01-25 23:00:00+00:00       521    PM1   7.95  2021-01-25  23:00:00
     3852 2021-01-25 23:00:00+00:00       521  PM2.5  12.65  2021-01-25  23:00:00
     3853 2021-01-25 23:00:00+00:00       521   PM10  27.28  2021-01-25  23:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3857 2021-01-25 23:10:01+00:00       521    PM1   7.64  2021-01-25  23:10:01
     3858 2021-01-25 23:10:01+00:00       521  PM2.5  12.17  2021-01-25  23:10:01
     3859 2021-01-25 23:10:01+00:00       521   PM10  21.79  2021-01-25  23:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     3863 2021-01-25 23:20:00+00:00       521    PM1   7.15  2021-01-25  23:20:00
     3864 2021-01-25 23:20:00+00:00       521  PM2.5  11.10  2021-01-25  23:20:00
     3865 2021-01-25 23:20:00+00:00       521   PM10  23.37  2021-01-25  23:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3869 2021-01-25 23:30:00+00:00       521    PM1   8.13  2021-01-25  23:30:00
     3870 2021-01-25 23:30:00+00:00       521  PM2.5  11.97  2021-01-25  23:30:00
     3871 2021-01-25 23:30:00+00:00       521   PM10  22.73  2021-01-25  23:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3875 2021-01-25 23:40:00+00:00       521    PM1   7.72  2021-01-25  23:40:00
     3876 2021-01-25 23:40:00+00:00       521  PM2.5  12.64  2021-01-25  23:40:00
     3877 2021-01-25 23:40:00+00:00       521   PM10  22.75  2021-01-25  23:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3881 2021-01-25 23:50:00+00:00       521    PM1   8.04  2021-01-25  23:50:00
     3882 2021-01-25 23:50:00+00:00       521  PM2.5  12.43  2021-01-25  23:50:00
     3883 2021-01-25 23:50:00+00:00       521   PM10  32.31  2021-01-25  23:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3887 2021-01-26 00:00:00+00:00       521    PM1   7.90  2021-01-26  00:00:00
     3888 2021-01-26 00:00:00+00:00       521  PM2.5  12.78  2021-01-26  00:00:00
     3889 2021-01-26 00:00:00+00:00       521   PM10  30.27  2021-01-26  00:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3893 2021-01-26 00:10:00+00:00       521    PM1   7.16  2021-01-26  00:10:00
     3894 2021-01-26 00:10:00+00:00       521  PM2.5  10.84  2021-01-26  00:10:00
     3895 2021-01-26 00:10:00+00:00       521   PM10  22.13  2021-01-26  00:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3899 2021-01-26 00:20:00+00:00       521    PM1   7.28  2021-01-26  00:20:00
     3900 2021-01-26 00:20:00+00:00       521  PM2.5  11.47  2021-01-26  00:20:00
     3901 2021-01-26 00:20:00+00:00       521   PM10  21.00  2021-01-26  00:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3905 2021-01-26 00:30:00+00:00       521    PM1   6.65  2021-01-26  00:30:00
     3906 2021-01-26 00:30:00+00:00       521  PM2.5  10.00  2021-01-26  00:30:00
     3907 2021-01-26 00:30:00+00:00       521   PM10  23.42  2021-01-26  00:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3911 2021-01-26 00:40:00+00:00       521    PM1   6.27  2021-01-26  00:40:00
     3912 2021-01-26 00:40:00+00:00       521  PM2.5   9.99  2021-01-26  00:40:00
     3913 2021-01-26 00:40:00+00:00       521   PM10  21.30  2021-01-26  00:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3917 2021-01-26 00:50:00+00:00       521    PM1   6.73  2021-01-26  00:50:00
     3918 2021-01-26 00:50:00+00:00       521  PM2.5  10.55  2021-01-26  00:50:00
     3919 2021-01-26 00:50:00+00:00       521   PM10  25.62  2021-01-26  00:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3923 2021-01-26 01:00:00+00:00       521    PM1   7.53  2021-01-26  01:00:00
     3924 2021-01-26 01:00:00+00:00       521  PM2.5  10.98  2021-01-26  01:00:00
     3925 2021-01-26 01:00:00+00:00       521   PM10  20.76  2021-01-26  01:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3929 2021-01-26 01:10:00+00:00       521    PM1   6.60  2021-01-26  01:10:00
     3930 2021-01-26 01:10:00+00:00       521  PM2.5   9.37  2021-01-26  01:10:00
     3931 2021-01-26 01:10:00+00:00       521   PM10  22.37  2021-01-26  01:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3935 2021-01-26 01:20:01+00:00       521    PM1   6.53  2021-01-26  01:20:01
     3936 2021-01-26 01:20:01+00:00       521  PM2.5   9.85  2021-01-26  01:20:01
     3937 2021-01-26 01:20:01+00:00       521   PM10  27.94  2021-01-26  01:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     3941 2021-01-26 01:30:00+00:00       521    PM1   5.92  2021-01-26  01:30:00
     3942 2021-01-26 01:30:00+00:00       521  PM2.5   9.47  2021-01-26  01:30:00
     3943 2021-01-26 01:30:00+00:00       521   PM10  20.15  2021-01-26  01:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3947 2021-01-26 01:40:00+00:00       521    PM1   6.22  2021-01-26  01:40:00
     3948 2021-01-26 01:40:00+00:00       521  PM2.5   8.64  2021-01-26  01:40:00
     3949 2021-01-26 01:40:00+00:00       521   PM10  17.36  2021-01-26  01:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3953 2021-01-26 01:50:00+00:00       521    PM1   6.98  2021-01-26  01:50:00
     3954 2021-01-26 01:50:00+00:00       521  PM2.5  10.18  2021-01-26  01:50:00
     3955 2021-01-26 01:50:00+00:00       521   PM10  18.19  2021-01-26  01:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     3959 2021-01-26 02:00:00+00:00       521    PM1   6.66  2021-01-26  02:00:00
     3960 2021-01-26 02:00:00+00:00       521  PM2.5  11.07  2021-01-26  02:00:00
     3961 2021-01-26 02:00:00+00:00       521   PM10  21.21  2021-01-26  02:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     3965 2021-01-26 02:10:00+00:00       521    PM1   5.13  2021-01-26  02:10:00
     3966 2021-01-26 02:10:00+00:00       521  PM2.5   7.91  2021-01-26  02:10:00
     3967 2021-01-26 02:10:00+00:00       521   PM10  18.97  2021-01-26  02:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     3971 2021-01-26 02:20:00+00:00       521    PM1   5.81  2021-01-26  02:20:00
     3972 2021-01-26 02:20:00+00:00       521  PM2.5   8.67  2021-01-26  02:20:00
     3973 2021-01-26 02:20:00+00:00       521   PM10  18.61  2021-01-26  02:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     3977 2021-01-26 02:30:00+00:00       521    PM1   6.82  2021-01-26  02:30:00
     3978 2021-01-26 02:30:00+00:00       521  PM2.5   9.41  2021-01-26  02:30:00
     3979 2021-01-26 02:30:00+00:00       521   PM10  18.96  2021-01-26  02:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     3983 2021-01-26 02:40:00+00:00       521    PM1   5.79  2021-01-26  02:40:00
     3984 2021-01-26 02:40:00+00:00       521  PM2.5   9.62  2021-01-26  02:40:00
     3985 2021-01-26 02:40:00+00:00       521   PM10  19.85  2021-01-26  02:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     3989 2021-01-26 02:50:01+00:00       521    PM1   4.92  2021-01-26  02:50:01
     3990 2021-01-26 02:50:01+00:00       521  PM2.5   8.22  2021-01-26  02:50:01
     3991 2021-01-26 02:50:01+00:00       521   PM10  19.25  2021-01-26  02:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     3995 2021-01-26 03:00:00+00:00       521    PM1   4.39  2021-01-26  03:00:00
     3996 2021-01-26 03:00:00+00:00       521  PM2.5   6.68  2021-01-26  03:00:00
     3997 2021-01-26 03:00:00+00:00       521   PM10  13.72  2021-01-26  03:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4001 2021-01-26 03:10:00+00:00       521    PM1   5.97  2021-01-26  03:10:00
     4002 2021-01-26 03:10:00+00:00       521  PM2.5   8.56  2021-01-26  03:10:00
     4003 2021-01-26 03:10:00+00:00       521   PM10  18.80  2021-01-26  03:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4007 2021-01-26 03:20:00+00:00       521    PM1   6.68  2021-01-26  03:20:00
     4008 2021-01-26 03:20:00+00:00       521  PM2.5  10.25  2021-01-26  03:20:00
     4009 2021-01-26 03:20:00+00:00       521   PM10  18.50  2021-01-26  03:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4013 2021-01-26 03:30:00+00:00       521    PM1   5.32  2021-01-26  03:30:00
     4014 2021-01-26 03:30:00+00:00       521  PM2.5   8.17  2021-01-26  03:30:00
     4015 2021-01-26 03:30:00+00:00       521   PM10  19.74  2021-01-26  03:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4022 2021-01-26 03:50:00+00:00       521    PM1   3.82  2021-01-26  03:50:00
     4023 2021-01-26 03:50:00+00:00       521  PM2.5   6.25  2021-01-26  03:50:00
     4024 2021-01-26 03:50:00+00:00       521   PM10  15.54  2021-01-26  03:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4028 2021-01-26 04:00:00+00:00       521    PM1   4.47  2021-01-26  04:00:00
     4029 2021-01-26 04:00:00+00:00       521  PM2.5   6.92  2021-01-26  04:00:00
     4030 2021-01-26 04:00:00+00:00       521   PM10  14.90  2021-01-26  04:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4034 2021-01-26 04:10:00+00:00       521    PM1   4.80  2021-01-26  04:10:00
     4035 2021-01-26 04:10:00+00:00       521  PM2.5   6.63  2021-01-26  04:10:00
     4036 2021-01-26 04:10:00+00:00       521   PM10  11.57  2021-01-26  04:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4040 2021-01-26 04:20:00+00:00       521    PM1   4.99  2021-01-26  04:20:00
     4041 2021-01-26 04:20:00+00:00       521  PM2.5   7.20  2021-01-26  04:20:00
     4042 2021-01-26 04:20:00+00:00       521   PM10  16.43  2021-01-26  04:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4046 2021-01-26 04:30:00+00:00       521    PM1   3.07  2021-01-26  04:30:00
     4047 2021-01-26 04:30:00+00:00       521  PM2.5   4.69  2021-01-26  04:30:00
     4048 2021-01-26 04:30:00+00:00       521   PM10   8.57  2021-01-26  04:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4052 2021-01-26 04:40:00+00:00       521    PM1   3.38  2021-01-26  04:40:00
     4053 2021-01-26 04:40:00+00:00       521  PM2.5   4.27  2021-01-26  04:40:00
     4054 2021-01-26 04:40:00+00:00       521   PM10  11.74  2021-01-26  04:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4058 2021-01-26 04:50:00+00:00       521    PM1   3.81  2021-01-26  04:50:00
     4059 2021-01-26 04:50:00+00:00       521  PM2.5   5.94  2021-01-26  04:50:00
     4060 2021-01-26 04:50:00+00:00       521   PM10  13.74  2021-01-26  04:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4064 2021-01-26 05:00:00+00:00       521    PM1   4.50  2021-01-26  05:00:00
     4065 2021-01-26 05:00:00+00:00       521  PM2.5   5.75  2021-01-26  05:00:00
     4066 2021-01-26 05:00:00+00:00       521   PM10  11.40  2021-01-26  05:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4070 2021-01-26 05:10:00+00:00       521    PM1   4.22  2021-01-26  05:10:00
     4071 2021-01-26 05:10:00+00:00       521  PM2.5   6.14  2021-01-26  05:10:00
     4072 2021-01-26 05:10:00+00:00       521   PM10  10.85  2021-01-26  05:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4076 2021-01-26 05:20:01+00:00       521    PM1   4.13  2021-01-26  05:20:01
     4077 2021-01-26 05:20:01+00:00       521  PM2.5   5.81  2021-01-26  05:20:01
     4078 2021-01-26 05:20:01+00:00       521   PM10  12.15  2021-01-26  05:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     4082 2021-01-26 05:30:00+00:00       521    PM1   3.11  2021-01-26  05:30:00
     4083 2021-01-26 05:30:00+00:00       521  PM2.5   4.50  2021-01-26  05:30:00
     4084 2021-01-26 05:30:00+00:00       521   PM10   9.68  2021-01-26  05:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4088 2021-01-26 05:40:00+00:00       521    PM1   3.01  2021-01-26  05:40:00
     4089 2021-01-26 05:40:00+00:00       521  PM2.5   4.36  2021-01-26  05:40:00
     4090 2021-01-26 05:40:00+00:00       521   PM10  10.38  2021-01-26  05:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4094 2021-01-26 05:50:00+00:00       521    PM1   3.64  2021-01-26  05:50:00
     4095 2021-01-26 05:50:00+00:00       521  PM2.5   4.71  2021-01-26  05:50:00
     4096 2021-01-26 05:50:00+00:00       521   PM10  11.41  2021-01-26  05:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4100 2021-01-26 06:00:00+00:00       521    PM1   4.60  2021-01-26  06:00:00
     4101 2021-01-26 06:00:00+00:00       521  PM2.5   6.74  2021-01-26  06:00:00
     4102 2021-01-26 06:00:00+00:00       521   PM10  11.58  2021-01-26  06:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4106 2021-01-26 06:10:00+00:00       521    PM1   4.31  2021-01-26  06:10:00
     4107 2021-01-26 06:10:00+00:00       521  PM2.5   5.60  2021-01-26  06:10:00
     4108 2021-01-26 06:10:00+00:00       521   PM10  12.92  2021-01-26  06:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4112 2021-01-26 06:20:00+00:00       521    PM1   4.59  2021-01-26  06:20:00
     4113 2021-01-26 06:20:00+00:00       521  PM2.5   6.26  2021-01-26  06:20:00
     4114 2021-01-26 06:20:00+00:00       521   PM10  12.68  2021-01-26  06:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4118 2021-01-26 06:30:00+00:00       521    PM1   4.51  2021-01-26  06:30:00
     4119 2021-01-26 06:30:00+00:00       521  PM2.5   6.05  2021-01-26  06:30:00
     4120 2021-01-26 06:30:00+00:00       521   PM10  12.99  2021-01-26  06:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4124 2021-01-26 06:40:01+00:00       521    PM1   4.25  2021-01-26  06:40:01
     4125 2021-01-26 06:40:01+00:00       521  PM2.5   6.39  2021-01-26  06:40:01
     4126 2021-01-26 06:40:01+00:00       521   PM10  12.94  2021-01-26  06:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     4130 2021-01-26 06:50:00+00:00       521    PM1   4.44  2021-01-26  06:50:00
     4131 2021-01-26 06:50:00+00:00       521  PM2.5   6.00  2021-01-26  06:50:00
     4132 2021-01-26 06:50:00+00:00       521   PM10  13.29  2021-01-26  06:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4136 2021-01-26 07:00:00+00:00       521    PM1   4.60  2021-01-26  07:00:00
     4137 2021-01-26 07:00:00+00:00       521  PM2.5   7.34  2021-01-26  07:00:00
     4138 2021-01-26 07:00:00+00:00       521   PM10  16.24  2021-01-26  07:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4142 2021-01-26 07:10:00+00:00       521    PM1   4.42  2021-01-26  07:10:00
     4143 2021-01-26 07:10:00+00:00       521  PM2.5   6.49  2021-01-26  07:10:00
     4144 2021-01-26 07:10:00+00:00       521   PM10  16.77  2021-01-26  07:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4148 2021-01-26 07:20:00+00:00       521    PM1   5.40  2021-01-26  07:20:00
     4149 2021-01-26 07:20:00+00:00       521  PM2.5   8.12  2021-01-26  07:20:00
     4150 2021-01-26 07:20:00+00:00       521   PM10  16.61  2021-01-26  07:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4154 2021-01-26 07:30:00+00:00       521    PM1   4.88  2021-01-26  07:30:00
     4155 2021-01-26 07:30:00+00:00       521  PM2.5   7.06  2021-01-26  07:30:00
     4156 2021-01-26 07:30:00+00:00       521   PM10  13.34  2021-01-26  07:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4160 2021-01-26 07:40:00+00:00       521    PM1   4.71  2021-01-26  07:40:00
     4161 2021-01-26 07:40:00+00:00       521  PM2.5   7.42  2021-01-26  07:40:00
     4162 2021-01-26 07:40:00+00:00       521   PM10  17.33  2021-01-26  07:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4166 2021-01-26 07:50:00+00:00       521    PM1   5.40  2021-01-26  07:50:00
     4167 2021-01-26 07:50:00+00:00       521  PM2.5   7.38  2021-01-26  07:50:00
     4168 2021-01-26 07:50:00+00:00       521   PM10  14.52  2021-01-26  07:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4172 2021-01-26 08:00:00+00:00       521    PM1   5.56  2021-01-26  08:00:00
     4173 2021-01-26 08:00:00+00:00       521  PM2.5   7.99  2021-01-26  08:00:00
     4174 2021-01-26 08:00:00+00:00       521   PM10  15.38  2021-01-26  08:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4178 2021-01-26 08:10:00+00:00       521    PM1   5.86  2021-01-26  08:10:00
     4179 2021-01-26 08:10:00+00:00       521  PM2.5   8.05  2021-01-26  08:10:00
     4180 2021-01-26 08:10:00+00:00       521   PM10  20.63  2021-01-26  08:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4184 2021-01-26 08:20:00+00:00       521    PM1   6.13  2021-01-26  08:20:00
     4185 2021-01-26 08:20:00+00:00       521  PM2.5   8.97  2021-01-26  08:20:00
     4186 2021-01-26 08:20:00+00:00       521   PM10  12.92  2021-01-26  08:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4190 2021-01-26 08:30:00+00:00       521    PM1   6.63  2021-01-26  08:30:00
     4191 2021-01-26 08:30:00+00:00       521  PM2.5   9.42  2021-01-26  08:30:00
     4192 2021-01-26 08:30:00+00:00       521   PM10  17.07  2021-01-26  08:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4196 2021-01-26 08:40:01+00:00       521    PM1   6.53  2021-01-26  08:40:01
     4197 2021-01-26 08:40:01+00:00       521  PM2.5   8.83  2021-01-26  08:40:01
     4198 2021-01-26 08:40:01+00:00       521   PM10  14.13  2021-01-26  08:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     4202 2021-01-26 08:50:00+00:00       521    PM1   7.41  2021-01-26  08:50:00
     4203 2021-01-26 08:50:00+00:00       521  PM2.5  10.38  2021-01-26  08:50:00
     4204 2021-01-26 08:50:00+00:00       521   PM10  19.26  2021-01-26  08:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4208 2021-01-26 09:00:00+00:00       521    PM1   5.59  2021-01-26  09:00:00
     4209 2021-01-26 09:00:00+00:00       521  PM2.5   8.37  2021-01-26  09:00:00
     4210 2021-01-26 09:00:00+00:00       521   PM10  16.46  2021-01-26  09:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4214 2021-01-26 09:10:00+00:00       521    PM1   5.04  2021-01-26  09:10:00
     4215 2021-01-26 09:10:00+00:00       521  PM2.5   7.25  2021-01-26  09:10:00
     4216 2021-01-26 09:10:00+00:00       521   PM10  16.25  2021-01-26  09:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4220 2021-01-26 09:20:00+00:00       521    PM1   4.49  2021-01-26  09:20:00
     4221 2021-01-26 09:20:00+00:00       521  PM2.5   6.55  2021-01-26  09:20:00
     4222 2021-01-26 09:20:00+00:00       521   PM10  12.80  2021-01-26  09:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4226 2021-01-26 09:30:00+00:00       521    PM1   5.86  2021-01-26  09:30:00
     4227 2021-01-26 09:30:00+00:00       521  PM2.5   8.15  2021-01-26  09:30:00
     4228 2021-01-26 09:30:00+00:00       521   PM10  12.23  2021-01-26  09:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4232 2021-01-26 09:40:00+00:00       521    PM1   5.60  2021-01-26  09:40:00
     4233 2021-01-26 09:40:00+00:00       521  PM2.5   7.56  2021-01-26  09:40:00
     4234 2021-01-26 09:40:00+00:00       521   PM10  18.32  2021-01-26  09:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4238 2021-01-26 09:50:00+00:00       521    PM1   5.01  2021-01-26  09:50:00
     4239 2021-01-26 09:50:00+00:00       521  PM2.5   6.85  2021-01-26  09:50:00
     4240 2021-01-26 09:50:00+00:00       521   PM10  12.96  2021-01-26  09:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4244 2021-01-26 10:00:00+00:00       521    PM1   5.09  2021-01-26  10:00:00
     4245 2021-01-26 10:00:00+00:00       521  PM2.5   7.38  2021-01-26  10:00:00
     4246 2021-01-26 10:00:00+00:00       521   PM10  13.65  2021-01-26  10:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4250 2021-01-26 10:10:00+00:00       521    PM1   4.09  2021-01-26  10:10:00
     4251 2021-01-26 10:10:00+00:00       521  PM2.5   6.07  2021-01-26  10:10:00
     4252 2021-01-26 10:10:00+00:00       521   PM10  13.10  2021-01-26  10:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4256 2021-01-26 10:20:00+00:00       521    PM1   4.59  2021-01-26  10:20:00
     4257 2021-01-26 10:20:00+00:00       521  PM2.5   5.86  2021-01-26  10:20:00
     4258 2021-01-26 10:20:00+00:00       521   PM10  12.45  2021-01-26  10:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4262 2021-01-26 10:30:00+00:00       521    PM1   3.65  2021-01-26  10:30:00
     4263 2021-01-26 10:30:00+00:00       521  PM2.5   5.28  2021-01-26  10:30:00
     4264 2021-01-26 10:30:00+00:00       521   PM10  11.15  2021-01-26  10:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4268 2021-01-26 10:40:00+00:00       521    PM1   4.10  2021-01-26  10:40:00
     4269 2021-01-26 10:40:00+00:00       521  PM2.5   5.92  2021-01-26  10:40:00
     4270 2021-01-26 10:40:00+00:00       521   PM10  12.08  2021-01-26  10:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4274 2021-01-26 10:50:00+00:00       521    PM1   4.12  2021-01-26  10:50:00
     4275 2021-01-26 10:50:00+00:00       521  PM2.5   5.64  2021-01-26  10:50:00
     4276 2021-01-26 10:50:00+00:00       521   PM10  13.09  2021-01-26  10:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4280 2021-01-26 11:00:01+00:00       521    PM1   2.80  2021-01-26  11:00:01
     4281 2021-01-26 11:00:01+00:00       521  PM2.5   4.17  2021-01-26  11:00:01
     4282 2021-01-26 11:00:01+00:00       521   PM10  10.97  2021-01-26  11:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     4286 2021-01-26 11:10:00+00:00       521    PM1   2.75  2021-01-26  11:10:00
     4287 2021-01-26 11:10:00+00:00       521  PM2.5   3.87  2021-01-26  11:10:00
     4288 2021-01-26 11:10:00+00:00       521   PM10   7.94  2021-01-26  11:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4292 2021-01-26 11:20:00+00:00       521    PM1   3.55  2021-01-26  11:20:00
     4293 2021-01-26 11:20:00+00:00       521  PM2.5   4.55  2021-01-26  11:20:00
     4294 2021-01-26 11:20:00+00:00       521   PM10  10.37  2021-01-26  11:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4298 2021-01-26 11:30:00+00:00       521    PM1   3.03  2021-01-26  11:30:00
     4299 2021-01-26 11:30:00+00:00       521  PM2.5   4.38  2021-01-26  11:30:00
     4300 2021-01-26 11:30:00+00:00       521   PM10  12.42  2021-01-26  11:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4304 2021-01-26 11:40:00+00:00       521    PM1   3.07  2021-01-26  11:40:00
     4305 2021-01-26 11:40:00+00:00       521  PM2.5   4.22  2021-01-26  11:40:00
     4306 2021-01-26 11:40:00+00:00       521   PM10  10.24  2021-01-26  11:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4310 2021-01-26 11:50:00+00:00       521    PM1   3.54  2021-01-26  11:50:00
     4311 2021-01-26 11:50:00+00:00       521  PM2.5   4.61  2021-01-26  11:50:00
     4312 2021-01-26 11:50:00+00:00       521   PM10   8.57  2021-01-26  11:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4316 2021-01-26 12:00:01+00:00       521    PM1   3.64  2021-01-26  12:00:01
     4317 2021-01-26 12:00:01+00:00       521  PM2.5   5.21  2021-01-26  12:00:01
     4318 2021-01-26 12:00:01+00:00       521   PM10  16.65  2021-01-26  12:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     4322 2021-01-26 12:10:00+00:00       521    PM1   4.76  2021-01-26  12:10:00
     4323 2021-01-26 12:10:00+00:00       521  PM2.5   7.09  2021-01-26  12:10:00
     4324 2021-01-26 12:10:00+00:00       521   PM10  15.04  2021-01-26  12:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4328 2021-01-26 12:20:00+00:00       521    PM1   4.08  2021-01-26  12:20:00
     4329 2021-01-26 12:20:00+00:00       521  PM2.5   6.19  2021-01-26  12:20:00
     4330 2021-01-26 12:20:00+00:00       521   PM10  13.10  2021-01-26  12:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4334 2021-01-26 12:30:00+00:00       521    PM1   3.69  2021-01-26  12:30:00
     4335 2021-01-26 12:30:00+00:00       521  PM2.5   5.78  2021-01-26  12:30:00
     4336 2021-01-26 12:30:00+00:00       521   PM10  10.98  2021-01-26  12:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4340 2021-01-26 12:40:00+00:00       521    PM1   3.14  2021-01-26  12:40:00
     4341 2021-01-26 12:40:00+00:00       521  PM2.5   4.64  2021-01-26  12:40:00
     4342 2021-01-26 12:40:00+00:00       521   PM10  11.31  2021-01-26  12:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4346 2021-01-26 12:50:00+00:00       521    PM1   2.80  2021-01-26  12:50:00
     4347 2021-01-26 12:50:00+00:00       521  PM2.5   3.79  2021-01-26  12:50:00
     4348 2021-01-26 12:50:00+00:00       521   PM10   9.37  2021-01-26  12:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4352 2021-01-26 13:00:00+00:00       521    PM1   4.87  2021-01-26  13:00:00
     4353 2021-01-26 13:00:00+00:00       521  PM2.5   6.98  2021-01-26  13:00:00
     4354 2021-01-26 13:00:00+00:00       521   PM10  13.64  2021-01-26  13:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4358 2021-01-26 13:10:00+00:00       521    PM1   4.27  2021-01-26  13:10:00
     4359 2021-01-26 13:10:00+00:00       521  PM2.5   5.26  2021-01-26  13:10:00
     4360 2021-01-26 13:10:00+00:00       521   PM10  10.00  2021-01-26  13:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4364 2021-01-26 13:20:00+00:00       521    PM1   6.12  2021-01-26  13:20:00
     4365 2021-01-26 13:20:00+00:00       521  PM2.5   8.13  2021-01-26  13:20:00
     4366 2021-01-26 13:20:00+00:00       521   PM10  18.43  2021-01-26  13:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4370 2021-01-26 13:30:00+00:00       521    PM1   6.20  2021-01-26  13:30:00
     4371 2021-01-26 13:30:00+00:00       521  PM2.5   9.32  2021-01-26  13:30:00
     4372 2021-01-26 13:30:00+00:00       521   PM10  22.10  2021-01-26  13:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4376 2021-01-26 13:40:00+00:00       521    PM1   5.94  2021-01-26  13:40:00
     4377 2021-01-26 13:40:00+00:00       521  PM2.5   9.81  2021-01-26  13:40:00
     4378 2021-01-26 13:40:00+00:00       521   PM10  26.85  2021-01-26  13:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4382 2021-01-26 13:50:00+00:00       521    PM1   6.08  2021-01-26  13:50:00
     4383 2021-01-26 13:50:00+00:00       521  PM2.5   9.65  2021-01-26  13:50:00
     4384 2021-01-26 13:50:00+00:00       521   PM10  23.94  2021-01-26  13:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4388 2021-01-26 14:00:00+00:00       521    PM1   5.66  2021-01-26  14:00:00
     4389 2021-01-26 14:00:00+00:00       521  PM2.5   8.47  2021-01-26  14:00:00
     4390 2021-01-26 14:00:00+00:00       521   PM10  19.93  2021-01-26  14:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4394 2021-01-26 14:10:00+00:00       521    PM1   5.35  2021-01-26  14:10:00
     4395 2021-01-26 14:10:00+00:00       521  PM2.5   7.49  2021-01-26  14:10:00
     4396 2021-01-26 14:10:00+00:00       521   PM10  15.00  2021-01-26  14:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4400 2021-01-26 14:20:00+00:00       521    PM1   4.94  2021-01-26  14:20:00
     4401 2021-01-26 14:20:00+00:00       521  PM2.5   7.38  2021-01-26  14:20:00
     4402 2021-01-26 14:20:00+00:00       521   PM10  19.31  2021-01-26  14:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4406 2021-01-26 14:30:00+00:00       521    PM1   5.22  2021-01-26  14:30:00
     4407 2021-01-26 14:30:00+00:00       521  PM2.5   6.97  2021-01-26  14:30:00
     4408 2021-01-26 14:30:00+00:00       521   PM10  21.39  2021-01-26  14:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4412 2021-01-26 14:40:01+00:00       521    PM1   5.33  2021-01-26  14:40:01
     4413 2021-01-26 14:40:01+00:00       521  PM2.5   7.81  2021-01-26  14:40:01
     4414 2021-01-26 14:40:01+00:00       521   PM10  16.05  2021-01-26  14:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     4418 2021-01-26 14:50:00+00:00       521    PM1   4.49  2021-01-26  14:50:00
     4419 2021-01-26 14:50:00+00:00       521  PM2.5   7.19  2021-01-26  14:50:00
     4420 2021-01-26 14:50:00+00:00       521   PM10  20.99  2021-01-26  14:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4424 2021-01-26 15:00:00+00:00       521    PM1   4.27  2021-01-26  15:00:00
     4425 2021-01-26 15:00:00+00:00       521  PM2.5   6.44  2021-01-26  15:00:00
     4426 2021-01-26 15:00:00+00:00       521   PM10  17.41  2021-01-26  15:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4430 2021-01-26 15:10:00+00:00       521    PM1   4.68  2021-01-26  15:10:00
     4431 2021-01-26 15:10:00+00:00       521  PM2.5   6.62  2021-01-26  15:10:00
     4432 2021-01-26 15:10:00+00:00       521   PM10  15.81  2021-01-26  15:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4436 2021-01-26 15:20:00+00:00       521    PM1   4.60  2021-01-26  15:20:00
     4437 2021-01-26 15:20:00+00:00       521  PM2.5   6.49  2021-01-26  15:20:00
     4438 2021-01-26 15:20:00+00:00       521   PM10  15.08  2021-01-26  15:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4442 2021-01-26 15:30:00+00:00       521    PM1   5.22  2021-01-26  15:30:00
     4443 2021-01-26 15:30:00+00:00       521  PM2.5   7.05  2021-01-26  15:30:00
     4444 2021-01-26 15:30:00+00:00       521   PM10  13.43  2021-01-26  15:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4448 2021-01-26 15:40:00+00:00       521    PM1   6.25  2021-01-26  15:40:00
     4449 2021-01-26 15:40:00+00:00       521  PM2.5   8.68  2021-01-26  15:40:00
     4450 2021-01-26 15:40:00+00:00       521   PM10  16.57  2021-01-26  15:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4454 2021-01-26 15:50:00+00:00       521    PM1   5.81  2021-01-26  15:50:00
     4455 2021-01-26 15:50:00+00:00       521  PM2.5   8.67  2021-01-26  15:50:00
     4456 2021-01-26 15:50:00+00:00       521   PM10  17.80  2021-01-26  15:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4460 2021-01-26 16:00:01+00:00       521    PM1   6.78  2021-01-26  16:00:01
     4461 2021-01-26 16:00:01+00:00       521  PM2.5   8.64  2021-01-26  16:00:01
     4462 2021-01-26 16:00:01+00:00       521   PM10  15.71  2021-01-26  16:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     4466 2021-01-26 16:10:00+00:00       521    PM1   6.26  2021-01-26  16:10:00
     4467 2021-01-26 16:10:00+00:00       521  PM2.5   8.57  2021-01-26  16:10:00
     4468 2021-01-26 16:10:00+00:00       521   PM10  14.93  2021-01-26  16:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4472 2021-01-26 16:19:59+00:00       521    PM1   5.62  2021-01-26  16:19:59
     4473 2021-01-26 16:19:59+00:00       521  PM2.5   8.51  2021-01-26  16:19:59
     4474 2021-01-26 16:19:59+00:00       521   PM10  16.75  2021-01-26  16:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     4478 2021-01-26 16:30:01+00:00       521    PM1   4.99  2021-01-26  16:30:01
     4479 2021-01-26 16:30:01+00:00       521  PM2.5   6.82  2021-01-26  16:30:01
     4480 2021-01-26 16:30:01+00:00       521   PM10  14.03  2021-01-26  16:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     4484 2021-01-26 16:40:00+00:00       521    PM1   5.42  2021-01-26  16:40:00
     4485 2021-01-26 16:40:00+00:00       521  PM2.5   7.44  2021-01-26  16:40:00
     4486 2021-01-26 16:40:00+00:00       521   PM10  16.51  2021-01-26  16:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4490 2021-01-26 16:50:00+00:00       521    PM1   7.03  2021-01-26  16:50:00
     4491 2021-01-26 16:50:00+00:00       521  PM2.5   9.27  2021-01-26  16:50:00
     4492 2021-01-26 16:50:00+00:00       521   PM10  14.95  2021-01-26  16:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4496 2021-01-26 17:00:00+00:00       521    PM1   8.39  2021-01-26  17:00:00
     4497 2021-01-26 17:00:00+00:00       521  PM2.5  10.10  2021-01-26  17:00:00
     4498 2021-01-26 17:00:00+00:00       521   PM10  15.41  2021-01-26  17:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4502 2021-01-26 17:10:00+00:00       521    PM1   7.65  2021-01-26  17:10:00
     4503 2021-01-26 17:10:00+00:00       521  PM2.5   9.85  2021-01-26  17:10:00
     4504 2021-01-26 17:10:00+00:00       521   PM10  17.54  2021-01-26  17:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4508 2021-01-26 17:20:00+00:00       521    PM1   6.05  2021-01-26  17:20:00
     4509 2021-01-26 17:20:00+00:00       521  PM2.5   7.70  2021-01-26  17:20:00
     4510 2021-01-26 17:20:00+00:00       521   PM10  14.43  2021-01-26  17:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4514 2021-01-26 17:30:00+00:00       521    PM1   4.65  2021-01-26  17:30:00
     4515 2021-01-26 17:30:00+00:00       521  PM2.5   6.08  2021-01-26  17:30:00
     4516 2021-01-26 17:30:00+00:00       521   PM10  12.61  2021-01-26  17:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4520 2021-01-26 17:40:00+00:00       521    PM1   5.03  2021-01-26  17:40:00
     4521 2021-01-26 17:40:00+00:00       521  PM2.5   6.00  2021-01-26  17:40:00
     4522 2021-01-26 17:40:00+00:00       521   PM10  11.66  2021-01-26  17:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4526 2021-01-26 17:50:00+00:00       521    PM1   7.25  2021-01-26  17:50:00
     4527 2021-01-26 17:50:00+00:00       521  PM2.5   9.59  2021-01-26  17:50:00
     4528 2021-01-26 17:50:00+00:00       521   PM10  15.33  2021-01-26  17:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4532 2021-01-26 18:00:00+00:00       521    PM1   7.33  2021-01-26  18:00:00
     4533 2021-01-26 18:00:00+00:00       521  PM2.5   9.18  2021-01-26  18:00:00
     4534 2021-01-26 18:00:00+00:00       521   PM10  12.94  2021-01-26  18:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4538 2021-01-26 18:10:01+00:00       521    PM1   7.63  2021-01-26  18:10:01
     4539 2021-01-26 18:10:01+00:00       521  PM2.5   9.60  2021-01-26  18:10:01
     4540 2021-01-26 18:10:01+00:00       521   PM10  14.85  2021-01-26  18:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     4544 2021-01-26 18:20:00+00:00       521    PM1   7.31  2021-01-26  18:20:00
     4545 2021-01-26 18:20:00+00:00       521  PM2.5   8.90  2021-01-26  18:20:00
     4546 2021-01-26 18:20:00+00:00       521   PM10  12.56  2021-01-26  18:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4550 2021-01-26 18:30:00+00:00       521    PM1   5.54  2021-01-26  18:30:00
     4551 2021-01-26 18:30:00+00:00       521  PM2.5   6.95  2021-01-26  18:30:00
     4552 2021-01-26 18:30:00+00:00       521   PM10  11.03  2021-01-26  18:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4556 2021-01-26 18:40:00+00:00       521    PM1   5.22  2021-01-26  18:40:00
     4557 2021-01-26 18:40:00+00:00       521  PM2.5   6.45  2021-01-26  18:40:00
     4558 2021-01-26 18:40:00+00:00       521   PM10  12.83  2021-01-26  18:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4562 2021-01-26 18:50:00+00:00       521    PM1   7.06  2021-01-26  18:50:00
     4563 2021-01-26 18:50:00+00:00       521  PM2.5   9.47  2021-01-26  18:50:00
     4564 2021-01-26 18:50:00+00:00       521   PM10  17.83  2021-01-26  18:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4568 2021-01-26 19:00:00+00:00       521    PM1  10.78  2021-01-26  19:00:00
     4569 2021-01-26 19:00:00+00:00       521  PM2.5  14.44  2021-01-26  19:00:00
     4570 2021-01-26 19:00:00+00:00       521   PM10  22.91  2021-01-26  19:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4574 2021-01-26 19:10:00+00:00       521    PM1  13.58  2021-01-26  19:10:00
     4575 2021-01-26 19:10:00+00:00       521  PM2.5  17.83  2021-01-26  19:10:00
     4576 2021-01-26 19:10:00+00:00       521   PM10  26.68  2021-01-26  19:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4580 2021-01-26 19:20:00+00:00       521    PM1  11.30  2021-01-26  19:20:00
     4581 2021-01-26 19:20:00+00:00       521  PM2.5  14.67  2021-01-26  19:20:00
     4582 2021-01-26 19:20:00+00:00       521   PM10  30.23  2021-01-26  19:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4586 2021-01-26 19:30:00+00:00       521    PM1  13.34  2021-01-26  19:30:00
     4587 2021-01-26 19:30:00+00:00       521  PM2.5  18.00  2021-01-26  19:30:00
     4588 2021-01-26 19:30:00+00:00       521   PM10  30.19  2021-01-26  19:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4592 2021-01-26 19:40:00+00:00       521    PM1  10.07  2021-01-26  19:40:00
     4593 2021-01-26 19:40:00+00:00       521  PM2.5  14.19  2021-01-26  19:40:00
     4594 2021-01-26 19:40:00+00:00       521   PM10  27.65  2021-01-26  19:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4598 2021-01-26 19:50:00+00:00       521    PM1   8.58  2021-01-26  19:50:00
     4599 2021-01-26 19:50:00+00:00       521  PM2.5  12.33  2021-01-26  19:50:00
     4600 2021-01-26 19:50:00+00:00       521   PM10  24.52  2021-01-26  19:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4604 2021-01-26 20:00:00+00:00       521    PM1  11.51  2021-01-26  20:00:00
     4605 2021-01-26 20:00:00+00:00       521  PM2.5  16.01  2021-01-26  20:00:00
     4606 2021-01-26 20:00:00+00:00       521   PM10  28.48  2021-01-26  20:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4610 2021-01-26 20:10:01+00:00       521    PM1  10.44  2021-01-26  20:10:01
     4611 2021-01-26 20:10:01+00:00       521  PM2.5  14.07  2021-01-26  20:10:01
     4612 2021-01-26 20:10:01+00:00       521   PM10  24.84  2021-01-26  20:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     4616 2021-01-26 20:20:00+00:00       521    PM1   8.87  2021-01-26  20:20:00
     4617 2021-01-26 20:20:00+00:00       521  PM2.5  12.22  2021-01-26  20:20:00
     4618 2021-01-26 20:20:00+00:00       521   PM10  21.15  2021-01-26  20:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4622 2021-01-26 20:30:00+00:00       521    PM1   7.14  2021-01-26  20:30:00
     4623 2021-01-26 20:30:00+00:00       521  PM2.5  10.71  2021-01-26  20:30:00
     4624 2021-01-26 20:30:00+00:00       521   PM10  23.01  2021-01-26  20:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4628 2021-01-26 20:40:00+00:00       521    PM1   7.58  2021-01-26  20:40:00
     4629 2021-01-26 20:40:00+00:00       521  PM2.5  10.22  2021-01-26  20:40:00
     4630 2021-01-26 20:40:00+00:00       521   PM10  28.53  2021-01-26  20:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4634 2021-01-26 20:50:00+00:00       521    PM1   7.28  2021-01-26  20:50:00
     4635 2021-01-26 20:50:00+00:00       521  PM2.5   9.97  2021-01-26  20:50:00
     4636 2021-01-26 20:50:00+00:00       521   PM10  19.62  2021-01-26  20:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4640 2021-01-26 21:00:00+00:00       521    PM1   6.73  2021-01-26  21:00:00
     4641 2021-01-26 21:00:00+00:00       521  PM2.5   8.74  2021-01-26  21:00:00
     4642 2021-01-26 21:00:00+00:00       521   PM10  18.26  2021-01-26  21:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4646 2021-01-26 21:10:00+00:00       521    PM1   5.59  2021-01-26  21:10:00
     4647 2021-01-26 21:10:00+00:00       521  PM2.5   8.34  2021-01-26  21:10:00
     4648 2021-01-26 21:10:00+00:00       521   PM10  19.75  2021-01-26  21:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4652 2021-01-26 21:20:00+00:00       521    PM1   5.41  2021-01-26  21:20:00
     4653 2021-01-26 21:20:00+00:00       521  PM2.5   9.16  2021-01-26  21:20:00
     4654 2021-01-26 21:20:00+00:00       521   PM10  18.42  2021-01-26  21:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4658 2021-01-26 21:30:01+00:00       521    PM1   6.25  2021-01-26  21:30:01
     4659 2021-01-26 21:30:01+00:00       521  PM2.5   8.00  2021-01-26  21:30:01
     4660 2021-01-26 21:30:01+00:00       521   PM10  16.25  2021-01-26  21:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     4664 2021-01-26 21:40:00+00:00       521    PM1   7.02  2021-01-26  21:40:00
     4665 2021-01-26 21:40:00+00:00       521  PM2.5   9.75  2021-01-26  21:40:00
     4666 2021-01-26 21:40:00+00:00       521   PM10  22.86  2021-01-26  21:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4670 2021-01-26 21:50:00+00:00       521    PM1   7.66  2021-01-26  21:50:00
     4671 2021-01-26 21:50:00+00:00       521  PM2.5  10.43  2021-01-26  21:50:00
     4672 2021-01-26 21:50:00+00:00       521   PM10  22.01  2021-01-26  21:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4676 2021-01-26 22:00:00+00:00       521    PM1   9.07  2021-01-26  22:00:00
     4677 2021-01-26 22:00:00+00:00       521  PM2.5  12.81  2021-01-26  22:00:00
     4678 2021-01-26 22:00:00+00:00       521   PM10  25.92  2021-01-26  22:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4682 2021-01-26 22:10:00+00:00       521    PM1   8.68  2021-01-26  22:10:00
     4683 2021-01-26 22:10:00+00:00       521  PM2.5  12.12  2021-01-26  22:10:00
     4684 2021-01-26 22:10:00+00:00       521   PM10  19.28  2021-01-26  22:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4688 2021-01-26 22:20:00+00:00       521    PM1   8.05  2021-01-26  22:20:00
     4689 2021-01-26 22:20:00+00:00       521  PM2.5  10.25  2021-01-26  22:20:00
     4690 2021-01-26 22:20:00+00:00       521   PM10  19.88  2021-01-26  22:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4694 2021-01-26 22:30:00+00:00       521    PM1   9.62  2021-01-26  22:30:00
     4695 2021-01-26 22:30:00+00:00       521  PM2.5  11.02  2021-01-26  22:30:00
     4696 2021-01-26 22:30:00+00:00       521   PM10  18.82  2021-01-26  22:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4700 2021-01-26 22:40:00+00:00       521    PM1   9.84  2021-01-26  22:40:00
     4701 2021-01-26 22:40:00+00:00       521  PM2.5  11.40  2021-01-26  22:40:00
     4702 2021-01-26 22:40:00+00:00       521   PM10  19.67  2021-01-26  22:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4706 2021-01-26 22:50:00+00:00       521    PM1  10.76  2021-01-26  22:50:00
     4707 2021-01-26 22:50:00+00:00       521  PM2.5  12.09  2021-01-26  22:50:00
     4708 2021-01-26 22:50:00+00:00       521   PM10  18.00  2021-01-26  22:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4712 2021-01-26 23:00:00+00:00       521    PM1  10.93  2021-01-26  23:00:00
     4713 2021-01-26 23:00:00+00:00       521  PM2.5  12.61  2021-01-26  23:00:00
     4714 2021-01-26 23:00:00+00:00       521   PM10  19.06  2021-01-26  23:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4718 2021-01-26 23:10:00+00:00       521    PM1  10.26  2021-01-26  23:10:00
     4719 2021-01-26 23:10:00+00:00       521  PM2.5  12.99  2021-01-26  23:10:00
     4720 2021-01-26 23:10:00+00:00       521   PM10  19.48  2021-01-26  23:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4724 2021-01-26 23:20:00+00:00       521    PM1  11.76  2021-01-26  23:20:00
     4725 2021-01-26 23:20:00+00:00       521  PM2.5  13.89  2021-01-26  23:20:00
     4726 2021-01-26 23:20:00+00:00       521   PM10  19.25  2021-01-26  23:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4730 2021-01-26 23:30:00+00:00       521    PM1  10.49  2021-01-26  23:30:00
     4731 2021-01-26 23:30:00+00:00       521  PM2.5  12.41  2021-01-26  23:30:00
     4732 2021-01-26 23:30:00+00:00       521   PM10  18.16  2021-01-26  23:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4736 2021-01-26 23:40:00+00:00       521    PM1  10.00  2021-01-26  23:40:00
     4737 2021-01-26 23:40:00+00:00       521  PM2.5  11.15  2021-01-26  23:40:00
     4738 2021-01-26 23:40:00+00:00       521   PM10  17.91  2021-01-26  23:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4742 2021-01-26 23:50:00+00:00       521    PM1  10.19  2021-01-26  23:50:00
     4743 2021-01-26 23:50:00+00:00       521  PM2.5  11.55  2021-01-26  23:50:00
     4744 2021-01-26 23:50:00+00:00       521   PM10  14.85  2021-01-26  23:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4748 2021-01-27 00:00:00+00:00       521    PM1   9.91  2021-01-27  00:00:00
     4749 2021-01-27 00:00:00+00:00       521  PM2.5  11.19  2021-01-27  00:00:00
     4750 2021-01-27 00:00:00+00:00       521   PM10  15.52  2021-01-27  00:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4754 2021-01-27 00:10:01+00:00       521    PM1   9.11  2021-01-27  00:10:01
     4755 2021-01-27 00:10:01+00:00       521  PM2.5  10.73  2021-01-27  00:10:01
     4756 2021-01-27 00:10:01+00:00       521   PM10  13.96  2021-01-27  00:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     4760 2021-01-27 00:20:00+00:00       521    PM1  10.35  2021-01-27  00:20:00
     4761 2021-01-27 00:20:00+00:00       521  PM2.5  11.45  2021-01-27  00:20:00
     4762 2021-01-27 00:20:00+00:00       521   PM10  14.85  2021-01-27  00:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4766 2021-01-27 00:30:00+00:00       521    PM1  10.31  2021-01-27  00:30:00
     4767 2021-01-27 00:30:00+00:00       521  PM2.5  11.76  2021-01-27  00:30:00
     4768 2021-01-27 00:30:00+00:00       521   PM10  15.27  2021-01-27  00:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4772 2021-01-27 00:40:00+00:00       521    PM1   9.98  2021-01-27  00:40:00
     4773 2021-01-27 00:40:00+00:00       521  PM2.5  11.18  2021-01-27  00:40:00
     4774 2021-01-27 00:40:00+00:00       521   PM10  14.98  2021-01-27  00:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4778 2021-01-27 00:50:00+00:00       521    PM1  10.78  2021-01-27  00:50:00
     4779 2021-01-27 00:50:00+00:00       521  PM2.5  12.39  2021-01-27  00:50:00
     4780 2021-01-27 00:50:00+00:00       521   PM10  15.73  2021-01-27  00:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4784 2021-01-27 01:00:00+00:00       521    PM1  11.45  2021-01-27  01:00:00
     4785 2021-01-27 01:00:00+00:00       521  PM2.5  12.74  2021-01-27  01:00:00
     4786 2021-01-27 01:00:00+00:00       521   PM10  18.07  2021-01-27  01:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4790 2021-01-27 01:10:01+00:00       521    PM1  11.59  2021-01-27  01:10:01
     4791 2021-01-27 01:10:01+00:00       521  PM2.5  12.85  2021-01-27  01:10:01
     4792 2021-01-27 01:10:01+00:00       521   PM10  16.67  2021-01-27  01:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     4796 2021-01-27 01:19:59+00:00       521    PM1  13.30  2021-01-27  01:19:59
     4797 2021-01-27 01:19:59+00:00       521  PM2.5  15.23  2021-01-27  01:19:59
     4798 2021-01-27 01:19:59+00:00       521   PM10  19.85  2021-01-27  01:19:59,
                          Timestamp  Location   Name  Value        Date      Time
     4802 2021-01-27 01:30:00+00:00       521    PM1  12.46  2021-01-27  01:30:00
     4803 2021-01-27 01:30:00+00:00       521  PM2.5  14.79  2021-01-27  01:30:00
     4804 2021-01-27 01:30:00+00:00       521   PM10  19.33  2021-01-27  01:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4808 2021-01-27 01:40:01+00:00       521    PM1  13.09  2021-01-27  01:40:01
     4809 2021-01-27 01:40:01+00:00       521  PM2.5  14.48  2021-01-27  01:40:01
     4810 2021-01-27 01:40:01+00:00       521   PM10  18.52  2021-01-27  01:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     4814 2021-01-27 01:49:59+00:00       521    PM1  11.96  2021-01-27  01:49:59
     4815 2021-01-27 01:49:59+00:00       521  PM2.5  13.89  2021-01-27  01:49:59
     4816 2021-01-27 01:49:59+00:00       521   PM10  15.63  2021-01-27  01:49:59,
                          Timestamp  Location   Name  Value        Date      Time
     4820 2021-01-27 02:00:01+00:00       521    PM1  11.72  2021-01-27  02:00:01
     4821 2021-01-27 02:00:01+00:00       521  PM2.5  12.88  2021-01-27  02:00:01
     4822 2021-01-27 02:00:01+00:00       521   PM10  16.23  2021-01-27  02:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     4826 2021-01-27 02:10:00+00:00       521    PM1  11.65  2021-01-27  02:10:00
     4827 2021-01-27 02:10:00+00:00       521  PM2.5  13.48  2021-01-27  02:10:00
     4828 2021-01-27 02:10:00+00:00       521   PM10  17.88  2021-01-27  02:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4832 2021-01-27 02:20:00+00:00       521    PM1  12.86  2021-01-27  02:20:00
     4833 2021-01-27 02:20:00+00:00       521  PM2.5  13.74  2021-01-27  02:20:00
     4834 2021-01-27 02:20:00+00:00       521   PM10  17.42  2021-01-27  02:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4838 2021-01-27 02:30:00+00:00       521    PM1  12.74  2021-01-27  02:30:00
     4839 2021-01-27 02:30:00+00:00       521  PM2.5  14.12  2021-01-27  02:30:00
     4840 2021-01-27 02:30:00+00:00       521   PM10  18.72  2021-01-27  02:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4844 2021-01-27 02:40:00+00:00       521    PM1  11.59  2021-01-27  02:40:00
     4845 2021-01-27 02:40:00+00:00       521  PM2.5  12.18  2021-01-27  02:40:00
     4846 2021-01-27 02:40:00+00:00       521   PM10  15.64  2021-01-27  02:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4850 2021-01-27 02:50:00+00:00       521    PM1  10.96  2021-01-27  02:50:00
     4851 2021-01-27 02:50:00+00:00       521  PM2.5  11.61  2021-01-27  02:50:00
     4852 2021-01-27 02:50:00+00:00       521   PM10  15.70  2021-01-27  02:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4856 2021-01-27 03:00:00+00:00       521    PM1   9.78  2021-01-27  03:00:00
     4857 2021-01-27 03:00:00+00:00       521  PM2.5  10.72  2021-01-27  03:00:00
     4858 2021-01-27 03:00:00+00:00       521   PM10  13.99  2021-01-27  03:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4862 2021-01-27 03:10:00+00:00       521    PM1  10.22  2021-01-27  03:10:00
     4863 2021-01-27 03:10:00+00:00       521  PM2.5  11.13  2021-01-27  03:10:00
     4864 2021-01-27 03:10:00+00:00       521   PM10  14.21  2021-01-27  03:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4868 2021-01-27 03:20:00+00:00       521    PM1  10.32  2021-01-27  03:20:00
     4869 2021-01-27 03:20:00+00:00       521  PM2.5  10.90  2021-01-27  03:20:00
     4870 2021-01-27 03:20:00+00:00       521   PM10  14.32  2021-01-27  03:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4874 2021-01-27 03:30:00+00:00       521    PM1   9.67  2021-01-27  03:30:00
     4875 2021-01-27 03:30:00+00:00       521  PM2.5  10.71  2021-01-27  03:30:00
     4876 2021-01-27 03:30:00+00:00       521   PM10  13.58  2021-01-27  03:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4883 2021-01-27 03:50:01+00:00       521    PM1   9.58  2021-01-27  03:50:01
     4884 2021-01-27 03:50:01+00:00       521  PM2.5  10.25  2021-01-27  03:50:01
     4885 2021-01-27 03:50:01+00:00       521   PM10  13.36  2021-01-27  03:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     4889 2021-01-27 04:00:00+00:00       521    PM1   8.39  2021-01-27  04:00:00
     4890 2021-01-27 04:00:00+00:00       521  PM2.5   9.07  2021-01-27  04:00:00
     4891 2021-01-27 04:00:00+00:00       521   PM10  11.61  2021-01-27  04:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4895 2021-01-27 04:10:00+00:00       521    PM1   9.09  2021-01-27  04:10:00
     4896 2021-01-27 04:10:00+00:00       521  PM2.5   9.67  2021-01-27  04:10:00
     4897 2021-01-27 04:10:00+00:00       521   PM10  13.24  2021-01-27  04:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4901 2021-01-27 04:20:00+00:00       521    PM1   8.37  2021-01-27  04:20:00
     4902 2021-01-27 04:20:00+00:00       521  PM2.5   8.90  2021-01-27  04:20:00
     4903 2021-01-27 04:20:00+00:00       521   PM10  10.29  2021-01-27  04:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4907 2021-01-27 04:30:00+00:00       521    PM1   6.73  2021-01-27  04:30:00
     4908 2021-01-27 04:30:00+00:00       521  PM2.5   6.87  2021-01-27  04:30:00
     4909 2021-01-27 04:30:00+00:00       521   PM10   8.57  2021-01-27  04:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4913 2021-01-27 04:40:00+00:00       521    PM1   6.90  2021-01-27  04:40:00
     4914 2021-01-27 04:40:00+00:00       521  PM2.5   7.41  2021-01-27  04:40:00
     4915 2021-01-27 04:40:00+00:00       521   PM10   9.31  2021-01-27  04:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4919 2021-01-27 04:50:00+00:00       521    PM1   5.66  2021-01-27  04:50:00
     4920 2021-01-27 04:50:00+00:00       521  PM2.5   5.97  2021-01-27  04:50:00
     4921 2021-01-27 04:50:00+00:00       521   PM10   8.32  2021-01-27  04:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     4925 2021-01-27 05:00:00+00:00       521    PM1   5.42  2021-01-27  05:00:00
     4926 2021-01-27 05:00:00+00:00       521  PM2.5   5.59  2021-01-27  05:00:00
     4927 2021-01-27 05:00:00+00:00       521   PM10   7.45  2021-01-27  05:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4931 2021-01-27 05:10:00+00:00       521    PM1   5.00  2021-01-27  05:10:00
     4932 2021-01-27 05:10:00+00:00       521  PM2.5   5.27  2021-01-27  05:10:00
     4933 2021-01-27 05:10:00+00:00       521   PM10   7.60  2021-01-27  05:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4937 2021-01-27 05:20:01+00:00       521    PM1   4.80  2021-01-27  05:20:01
     4938 2021-01-27 05:20:01+00:00       521  PM2.5   5.01  2021-01-27  05:20:01
     4939 2021-01-27 05:20:01+00:00       521   PM10   6.32  2021-01-27  05:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     4943 2021-01-27 05:30:00+00:00       521    PM1   4.79  2021-01-27  05:30:00
     4944 2021-01-27 05:30:00+00:00       521  PM2.5   5.10  2021-01-27  05:30:00
     4945 2021-01-27 05:30:00+00:00       521   PM10   6.40  2021-01-27  05:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4949 2021-01-27 05:39:59+00:00       521    PM1   4.11  2021-01-27  05:39:59
     4950 2021-01-27 05:39:59+00:00       521  PM2.5   4.37  2021-01-27  05:39:59
     4951 2021-01-27 05:39:59+00:00       521   PM10   6.47  2021-01-27  05:39:59,
                          Timestamp  Location   Name  Value        Date      Time
     4955 2021-01-27 05:50:01+00:00       521    PM1   3.67  2021-01-27  05:50:01
     4956 2021-01-27 05:50:01+00:00       521  PM2.5   3.67  2021-01-27  05:50:01
     4957 2021-01-27 05:50:01+00:00       521   PM10   5.47  2021-01-27  05:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     4961 2021-01-27 06:00:00+00:00       521    PM1   4.04  2021-01-27  06:00:00
     4962 2021-01-27 06:00:00+00:00       521  PM2.5   4.04  2021-01-27  06:00:00
     4963 2021-01-27 06:00:00+00:00       521   PM10   6.19  2021-01-27  06:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     4967 2021-01-27 06:10:00+00:00       521    PM1   3.60  2021-01-27  06:10:00
     4968 2021-01-27 06:10:00+00:00       521  PM2.5   3.77  2021-01-27  06:10:00
     4969 2021-01-27 06:10:00+00:00       521   PM10   5.82  2021-01-27  06:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     4973 2021-01-27 06:20:00+00:00       521    PM1   3.71  2021-01-27  06:20:00
     4974 2021-01-27 06:20:00+00:00       521  PM2.5   3.94  2021-01-27  06:20:00
     4975 2021-01-27 06:20:00+00:00       521   PM10   5.39  2021-01-27  06:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     4979 2021-01-27 06:30:00+00:00       521    PM1   4.03  2021-01-27  06:30:00
     4980 2021-01-27 06:30:00+00:00       521  PM2.5   4.22  2021-01-27  06:30:00
     4981 2021-01-27 06:30:00+00:00       521   PM10   5.63  2021-01-27  06:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     4985 2021-01-27 06:40:00+00:00       521    PM1   3.77  2021-01-27  06:40:00
     4986 2021-01-27 06:40:00+00:00       521  PM2.5   3.99  2021-01-27  06:40:00
     4987 2021-01-27 06:40:00+00:00       521   PM10   5.43  2021-01-27  06:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     4991 2021-01-27 06:50:01+00:00       521    PM1   3.94  2021-01-27  06:50:01
     4992 2021-01-27 06:50:01+00:00       521  PM2.5   4.07  2021-01-27  06:50:01
     4993 2021-01-27 06:50:01+00:00       521   PM10   6.08  2021-01-27  06:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     4997 2021-01-27 07:00:00+00:00       521    PM1   4.41  2021-01-27  07:00:00
     4998 2021-01-27 07:00:00+00:00       521  PM2.5   4.64  2021-01-27  07:00:00
     4999 2021-01-27 07:00:00+00:00       521   PM10   6.62  2021-01-27  07:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5003 2021-01-27 07:10:00+00:00       521    PM1   5.06  2021-01-27  07:10:00
     5004 2021-01-27 07:10:00+00:00       521  PM2.5   5.22  2021-01-27  07:10:00
     5005 2021-01-27 07:10:00+00:00       521   PM10   7.70  2021-01-27  07:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5009 2021-01-27 07:20:00+00:00       521    PM1   5.23  2021-01-27  07:20:00
     5010 2021-01-27 07:20:00+00:00       521  PM2.5   5.52  2021-01-27  07:20:00
     5011 2021-01-27 07:20:00+00:00       521   PM10   8.29  2021-01-27  07:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5015 2021-01-27 07:30:00+00:00       521    PM1   5.03  2021-01-27  07:30:00
     5016 2021-01-27 07:30:00+00:00       521  PM2.5   5.43  2021-01-27  07:30:00
     5017 2021-01-27 07:30:00+00:00       521   PM10   8.12  2021-01-27  07:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5021 2021-01-27 07:40:00+00:00       521    PM1   4.97  2021-01-27  07:40:00
     5022 2021-01-27 07:40:00+00:00       521  PM2.5   5.56  2021-01-27  07:40:00
     5023 2021-01-27 07:40:00+00:00       521   PM10   7.38  2021-01-27  07:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5027 2021-01-27 07:50:00+00:00       521    PM1   6.34  2021-01-27  07:50:00
     5028 2021-01-27 07:50:00+00:00       521  PM2.5   7.17  2021-01-27  07:50:00
     5029 2021-01-27 07:50:00+00:00       521   PM10   9.57  2021-01-27  07:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5033 2021-01-27 08:00:00+00:00       521    PM1   7.83  2021-01-27  08:00:00
     5034 2021-01-27 08:00:00+00:00       521  PM2.5   8.48  2021-01-27  08:00:00
     5035 2021-01-27 08:00:00+00:00       521   PM10  12.47  2021-01-27  08:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5039 2021-01-27 08:10:00+00:00       521    PM1   9.77  2021-01-27  08:10:00
     5040 2021-01-27 08:10:00+00:00       521  PM2.5  10.81  2021-01-27  08:10:00
     5041 2021-01-27 08:10:00+00:00       521   PM10  13.96  2021-01-27  08:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5045 2021-01-27 08:20:00+00:00       521    PM1   6.80  2021-01-27  08:20:00
     5046 2021-01-27 08:20:00+00:00       521  PM2.5   7.43  2021-01-27  08:20:00
     5047 2021-01-27 08:20:00+00:00       521   PM10  10.87  2021-01-27  08:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5051 2021-01-27 08:30:00+00:00       521    PM1   5.47  2021-01-27  08:30:00
     5052 2021-01-27 08:30:00+00:00       521  PM2.5   6.06  2021-01-27  08:30:00
     5053 2021-01-27 08:30:00+00:00       521   PM10   8.21  2021-01-27  08:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5057 2021-01-27 08:40:00+00:00       521    PM1   5.64  2021-01-27  08:40:00
     5058 2021-01-27 08:40:00+00:00       521  PM2.5   6.15  2021-01-27  08:40:00
     5059 2021-01-27 08:40:00+00:00       521   PM10   8.97  2021-01-27  08:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5063 2021-01-27 08:50:00+00:00       521    PM1   5.28  2021-01-27  08:50:00
     5064 2021-01-27 08:50:00+00:00       521  PM2.5   5.78  2021-01-27  08:50:00
     5065 2021-01-27 08:50:00+00:00       521   PM10   8.20  2021-01-27  08:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5069 2021-01-27 09:00:00+00:00       521    PM1   5.44  2021-01-27  09:00:00
     5070 2021-01-27 09:00:00+00:00       521  PM2.5   6.33  2021-01-27  09:00:00
     5071 2021-01-27 09:00:00+00:00       521   PM10   8.39  2021-01-27  09:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5075 2021-01-27 09:10:01+00:00       521    PM1   4.10  2021-01-27  09:10:01
     5076 2021-01-27 09:10:01+00:00       521  PM2.5   5.29  2021-01-27  09:10:01
     5077 2021-01-27 09:10:01+00:00       521   PM10  10.21  2021-01-27  09:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     5081 2021-01-27 09:20:00+00:00       521    PM1   3.66  2021-01-27  09:20:00
     5082 2021-01-27 09:20:00+00:00       521  PM2.5   4.88  2021-01-27  09:20:00
     5083 2021-01-27 09:20:00+00:00       521   PM10  10.05  2021-01-27  09:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5087 2021-01-27 09:29:59+00:00       521    PM1   3.82  2021-01-27  09:29:59
     5088 2021-01-27 09:29:59+00:00       521  PM2.5   4.71  2021-01-27  09:29:59
     5089 2021-01-27 09:29:59+00:00       521   PM10  12.08  2021-01-27  09:29:59,
                          Timestamp  Location   Name  Value        Date      Time
     5093 2021-01-27 09:40:01+00:00       521    PM1   3.87  2021-01-27  09:40:01
     5094 2021-01-27 09:40:01+00:00       521  PM2.5   5.34  2021-01-27  09:40:01
     5095 2021-01-27 09:40:01+00:00       521   PM10   9.45  2021-01-27  09:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     5099 2021-01-27 09:50:00+00:00       521    PM1   4.11  2021-01-27  09:50:00
     5100 2021-01-27 09:50:00+00:00       521  PM2.5   5.61  2021-01-27  09:50:00
     5101 2021-01-27 09:50:00+00:00       521   PM10  10.59  2021-01-27  09:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5105 2021-01-27 10:00:00+00:00       521    PM1   4.73  2021-01-27  10:00:00
     5106 2021-01-27 10:00:00+00:00       521  PM2.5   6.70  2021-01-27  10:00:00
     5107 2021-01-27 10:00:00+00:00       521   PM10  12.65  2021-01-27  10:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5111 2021-01-27 10:10:00+00:00       521    PM1   4.96  2021-01-27  10:10:00
     5112 2021-01-27 10:10:00+00:00       521  PM2.5   6.11  2021-01-27  10:10:00
     5113 2021-01-27 10:10:00+00:00       521   PM10  11.06  2021-01-27  10:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5117 2021-01-27 10:20:00+00:00       521    PM1   5.18  2021-01-27  10:20:00
     5118 2021-01-27 10:20:00+00:00       521  PM2.5   6.48  2021-01-27  10:20:00
     5119 2021-01-27 10:20:00+00:00       521   PM10  11.84  2021-01-27  10:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5123 2021-01-27 10:30:01+00:00       521    PM1   5.14  2021-01-27  10:30:01
     5124 2021-01-27 10:30:01+00:00       521  PM2.5   7.33  2021-01-27  10:30:01
     5125 2021-01-27 10:30:01+00:00       521   PM10  11.67  2021-01-27  10:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     5129 2021-01-27 10:39:59+00:00       521    PM1   5.36  2021-01-27  10:39:59
     5130 2021-01-27 10:39:59+00:00       521  PM2.5   7.57  2021-01-27  10:39:59
     5131 2021-01-27 10:39:59+00:00       521   PM10  12.41  2021-01-27  10:39:59,
                          Timestamp  Location   Name  Value        Date      Time
     5135 2021-01-27 10:50:01+00:00       521    PM1   8.23  2021-01-27  10:50:01
     5136 2021-01-27 10:50:01+00:00       521  PM2.5  11.04  2021-01-27  10:50:01
     5137 2021-01-27 10:50:01+00:00       521   PM10  18.86  2021-01-27  10:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     5141 2021-01-27 11:00:00+00:00       521    PM1   9.58  2021-01-27  11:00:00
     5142 2021-01-27 11:00:00+00:00       521  PM2.5  12.22  2021-01-27  11:00:00
     5143 2021-01-27 11:00:00+00:00       521   PM10  17.65  2021-01-27  11:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5147 2021-01-27 11:10:00+00:00       521    PM1   8.31  2021-01-27  11:10:00
     5148 2021-01-27 11:10:00+00:00       521  PM2.5  10.23  2021-01-27  11:10:00
     5149 2021-01-27 11:10:00+00:00       521   PM10  19.53  2021-01-27  11:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5153 2021-01-27 11:20:00+00:00       521    PM1   5.77  2021-01-27  11:20:00
     5154 2021-01-27 11:20:00+00:00       521  PM2.5   8.18  2021-01-27  11:20:00
     5155 2021-01-27 11:20:00+00:00       521   PM10  13.77  2021-01-27  11:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5159 2021-01-27 11:30:00+00:00       521    PM1   6.32  2021-01-27  11:30:00
     5160 2021-01-27 11:30:00+00:00       521  PM2.5   8.24  2021-01-27  11:30:00
     5161 2021-01-27 11:30:00+00:00       521   PM10  12.06  2021-01-27  11:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5165 2021-01-27 11:40:00+00:00       521    PM1   6.92  2021-01-27  11:40:00
     5166 2021-01-27 11:40:00+00:00       521  PM2.5   8.15  2021-01-27  11:40:00
     5167 2021-01-27 11:40:00+00:00       521   PM10  17.32  2021-01-27  11:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5171 2021-01-27 11:50:00+00:00       521    PM1   6.62  2021-01-27  11:50:00
     5172 2021-01-27 11:50:00+00:00       521  PM2.5   8.45  2021-01-27  11:50:00
     5173 2021-01-27 11:50:00+00:00       521   PM10  16.91  2021-01-27  11:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5177 2021-01-27 12:00:00+00:00       521    PM1   7.78  2021-01-27  12:00:00
     5178 2021-01-27 12:00:00+00:00       521  PM2.5   9.81  2021-01-27  12:00:00
     5179 2021-01-27 12:00:00+00:00       521   PM10  19.34  2021-01-27  12:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5183 2021-01-27 12:10:00+00:00       521    PM1   5.19  2021-01-27  12:10:00
     5184 2021-01-27 12:10:00+00:00       521  PM2.5   6.88  2021-01-27  12:10:00
     5185 2021-01-27 12:10:00+00:00       521   PM10  14.66  2021-01-27  12:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5189 2021-01-27 12:20:00+00:00       521    PM1   6.21  2021-01-27  12:20:00
     5190 2021-01-27 12:20:00+00:00       521  PM2.5   8.83  2021-01-27  12:20:00
     5191 2021-01-27 12:20:00+00:00       521   PM10  16.44  2021-01-27  12:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5195 2021-01-27 12:30:00+00:00       521    PM1   6.61  2021-01-27  12:30:00
     5196 2021-01-27 12:30:00+00:00       521  PM2.5   8.50  2021-01-27  12:30:00
     5197 2021-01-27 12:30:00+00:00       521   PM10  13.28  2021-01-27  12:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5201 2021-01-27 12:40:00+00:00       521    PM1   6.30  2021-01-27  12:40:00
     5202 2021-01-27 12:40:00+00:00       521  PM2.5   8.68  2021-01-27  12:40:00
     5203 2021-01-27 12:40:00+00:00       521   PM10  16.60  2021-01-27  12:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5207 2021-01-27 12:50:00+00:00       521    PM1   6.76  2021-01-27  12:50:00
     5208 2021-01-27 12:50:00+00:00       521  PM2.5   9.47  2021-01-27  12:50:00
     5209 2021-01-27 12:50:00+00:00       521   PM10  16.42  2021-01-27  12:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5213 2021-01-27 13:00:00+00:00       521    PM1   7.61  2021-01-27  13:00:00
     5214 2021-01-27 13:00:00+00:00       521  PM2.5  10.19  2021-01-27  13:00:00
     5215 2021-01-27 13:00:00+00:00       521   PM10  21.50  2021-01-27  13:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5219 2021-01-27 13:10:01+00:00       521    PM1  11.15  2021-01-27  13:10:01
     5220 2021-01-27 13:10:01+00:00       521  PM2.5  13.75  2021-01-27  13:10:01
     5221 2021-01-27 13:10:01+00:00       521   PM10  22.89  2021-01-27  13:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     5225 2021-01-27 13:20:00+00:00       521    PM1  10.36  2021-01-27  13:20:00
     5226 2021-01-27 13:20:00+00:00       521  PM2.5  12.80  2021-01-27  13:20:00
     5227 2021-01-27 13:20:00+00:00       521   PM10  23.69  2021-01-27  13:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5231 2021-01-27 13:30:00+00:00       521    PM1   9.01  2021-01-27  13:30:00
     5232 2021-01-27 13:30:00+00:00       521  PM2.5  12.59  2021-01-27  13:30:00
     5233 2021-01-27 13:30:00+00:00       521   PM10  22.47  2021-01-27  13:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5237 2021-01-27 13:40:00+00:00       521    PM1   9.74  2021-01-27  13:40:00
     5238 2021-01-27 13:40:00+00:00       521  PM2.5  12.76  2021-01-27  13:40:00
     5239 2021-01-27 13:40:00+00:00       521   PM10  20.30  2021-01-27  13:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5243 2021-01-27 13:50:00+00:00       521    PM1   9.65  2021-01-27  13:50:00
     5244 2021-01-27 13:50:00+00:00       521  PM2.5  11.95  2021-01-27  13:50:00
     5245 2021-01-27 13:50:00+00:00       521   PM10  24.93  2021-01-27  13:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5249 2021-01-27 14:00:00+00:00       521    PM1   8.56  2021-01-27  14:00:00
     5250 2021-01-27 14:00:00+00:00       521  PM2.5  11.14  2021-01-27  14:00:00
     5251 2021-01-27 14:00:00+00:00       521   PM10  21.64  2021-01-27  14:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5255 2021-01-27 14:10:00+00:00       521    PM1   7.93  2021-01-27  14:10:00
     5256 2021-01-27 14:10:00+00:00       521  PM2.5  10.19  2021-01-27  14:10:00
     5257 2021-01-27 14:10:00+00:00       521   PM10  20.29  2021-01-27  14:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5261 2021-01-27 14:20:00+00:00       521    PM1   9.70  2021-01-27  14:20:00
     5262 2021-01-27 14:20:00+00:00       521  PM2.5  12.45  2021-01-27  14:20:00
     5263 2021-01-27 14:20:00+00:00       521   PM10  22.04  2021-01-27  14:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5267 2021-01-27 14:30:00+00:00       521    PM1  10.21  2021-01-27  14:30:00
     5268 2021-01-27 14:30:00+00:00       521  PM2.5  14.07  2021-01-27  14:30:00
     5269 2021-01-27 14:30:00+00:00       521   PM10  23.94  2021-01-27  14:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5273 2021-01-27 14:40:01+00:00       521    PM1  12.26  2021-01-27  14:40:01
     5274 2021-01-27 14:40:01+00:00       521  PM2.5  15.57  2021-01-27  14:40:01
     5275 2021-01-27 14:40:01+00:00       521   PM10  26.34  2021-01-27  14:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     5279 2021-01-27 14:50:00+00:00       521    PM1  13.09  2021-01-27  14:50:00
     5280 2021-01-27 14:50:00+00:00       521  PM2.5  15.77  2021-01-27  14:50:00
     5281 2021-01-27 14:50:00+00:00       521   PM10  26.59  2021-01-27  14:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5285 2021-01-27 15:00:00+00:00       521    PM1  14.09  2021-01-27  15:00:00
     5286 2021-01-27 15:00:00+00:00       521  PM2.5  17.41  2021-01-27  15:00:00
     5287 2021-01-27 15:00:00+00:00       521   PM10  27.18  2021-01-27  15:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5291 2021-01-27 15:10:00+00:00       521    PM1  14.50  2021-01-27  15:10:00
     5292 2021-01-27 15:10:00+00:00       521  PM2.5  18.05  2021-01-27  15:10:00
     5293 2021-01-27 15:10:00+00:00       521   PM10  32.43  2021-01-27  15:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5297 2021-01-27 15:20:00+00:00       521    PM1  15.82  2021-01-27  15:20:00
     5298 2021-01-27 15:20:00+00:00       521  PM2.5  19.09  2021-01-27  15:20:00
     5299 2021-01-27 15:20:00+00:00       521   PM10  32.27  2021-01-27  15:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5303 2021-01-27 15:30:00+00:00       521    PM1  15.50  2021-01-27  15:30:00
     5304 2021-01-27 15:30:00+00:00       521  PM2.5  19.41  2021-01-27  15:30:00
     5305 2021-01-27 15:30:00+00:00       521   PM10  31.92  2021-01-27  15:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5309 2021-01-27 15:40:00+00:00       521    PM1  16.99  2021-01-27  15:40:00
     5310 2021-01-27 15:40:00+00:00       521  PM2.5  22.66  2021-01-27  15:40:00
     5311 2021-01-27 15:40:00+00:00       521   PM10  34.18  2021-01-27  15:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5315 2021-01-27 15:50:00+00:00       521    PM1  16.80  2021-01-27  15:50:00
     5316 2021-01-27 15:50:00+00:00       521  PM2.5  22.23  2021-01-27  15:50:00
     5317 2021-01-27 15:50:00+00:00       521   PM10  42.94  2021-01-27  15:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5321 2021-01-27 16:00:00+00:00       521    PM1  16.53  2021-01-27  16:00:00
     5322 2021-01-27 16:00:00+00:00       521  PM2.5  24.24  2021-01-27  16:00:00
     5323 2021-01-27 16:00:00+00:00       521   PM10  55.49  2021-01-27  16:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5327 2021-01-27 16:10:00+00:00       521    PM1  14.28  2021-01-27  16:10:00
     5328 2021-01-27 16:10:00+00:00       521  PM2.5  20.31  2021-01-27  16:10:00
     5329 2021-01-27 16:10:00+00:00       521   PM10  41.50  2021-01-27  16:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5333 2021-01-27 16:20:01+00:00       521    PM1  13.03  2021-01-27  16:20:01
     5334 2021-01-27 16:20:01+00:00       521  PM2.5  18.25  2021-01-27  16:20:01
     5335 2021-01-27 16:20:01+00:00       521   PM10  35.77  2021-01-27  16:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     5339 2021-01-27 16:30:00+00:00       521    PM1  13.67  2021-01-27  16:30:00
     5340 2021-01-27 16:30:00+00:00       521  PM2.5  19.14  2021-01-27  16:30:00
     5341 2021-01-27 16:30:00+00:00       521   PM10  40.19  2021-01-27  16:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5345 2021-01-27 16:40:00+00:00       521    PM1  13.73  2021-01-27  16:40:00
     5346 2021-01-27 16:40:00+00:00       521  PM2.5  18.44  2021-01-27  16:40:00
     5347 2021-01-27 16:40:00+00:00       521   PM10  43.50  2021-01-27  16:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5351 2021-01-27 16:50:00+00:00       521    PM1  13.75  2021-01-27  16:50:00
     5352 2021-01-27 16:50:00+00:00       521  PM2.5  18.76  2021-01-27  16:50:00
     5353 2021-01-27 16:50:00+00:00       521   PM10  40.45  2021-01-27  16:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5357 2021-01-27 17:00:00+00:00       521    PM1  13.73  2021-01-27  17:00:00
     5358 2021-01-27 17:00:00+00:00       521  PM2.5  20.61  2021-01-27  17:00:00
     5359 2021-01-27 17:00:00+00:00       521   PM10  39.88  2021-01-27  17:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5363 2021-01-27 17:10:00+00:00       521    PM1  11.87  2021-01-27  17:10:00
     5364 2021-01-27 17:10:00+00:00       521  PM2.5  19.43  2021-01-27  17:10:00
     5365 2021-01-27 17:10:00+00:00       521   PM10  37.62  2021-01-27  17:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5369 2021-01-27 17:20:00+00:00       521    PM1  12.24  2021-01-27  17:20:00
     5370 2021-01-27 17:20:00+00:00       521  PM2.5  18.72  2021-01-27  17:20:00
     5371 2021-01-27 17:20:00+00:00       521   PM10  38.76  2021-01-27  17:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5375 2021-01-27 17:30:00+00:00       521    PM1  13.11  2021-01-27  17:30:00
     5376 2021-01-27 17:30:00+00:00       521  PM2.5  20.08  2021-01-27  17:30:00
     5377 2021-01-27 17:30:00+00:00       521   PM10  45.09  2021-01-27  17:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5381 2021-01-27 17:40:00+00:00       521    PM1  12.73  2021-01-27  17:40:00
     5382 2021-01-27 17:40:00+00:00       521  PM2.5  19.20  2021-01-27  17:40:00
     5383 2021-01-27 17:40:00+00:00       521   PM10  44.99  2021-01-27  17:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5387 2021-01-27 17:50:00+00:00       521    PM1  12.50  2021-01-27  17:50:00
     5388 2021-01-27 17:50:00+00:00       521  PM2.5  18.66  2021-01-27  17:50:00
     5389 2021-01-27 17:50:00+00:00       521   PM10  43.04  2021-01-27  17:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5393 2021-01-27 18:00:00+00:00       521    PM1  11.46  2021-01-27  18:00:00
     5394 2021-01-27 18:00:00+00:00       521  PM2.5  19.13  2021-01-27  18:00:00
     5395 2021-01-27 18:00:00+00:00       521   PM10  43.35  2021-01-27  18:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5399 2021-01-27 18:10:00+00:00       521    PM1  12.16  2021-01-27  18:10:00
     5400 2021-01-27 18:10:00+00:00       521  PM2.5  19.52  2021-01-27  18:10:00
     5401 2021-01-27 18:10:00+00:00       521   PM10  42.46  2021-01-27  18:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5405 2021-01-27 18:20:00+00:00       521    PM1  13.39  2021-01-27  18:20:00
     5406 2021-01-27 18:20:00+00:00       521  PM2.5  20.85  2021-01-27  18:20:00
     5407 2021-01-27 18:20:00+00:00       521   PM10  37.75  2021-01-27  18:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5411 2021-01-27 18:30:01+00:00       521    PM1  12.30  2021-01-27  18:30:01
     5412 2021-01-27 18:30:01+00:00       521  PM2.5  19.49  2021-01-27  18:30:01
     5413 2021-01-27 18:30:01+00:00       521   PM10  45.10  2021-01-27  18:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     5417 2021-01-27 18:40:00+00:00       521    PM1  11.94  2021-01-27  18:40:00
     5418 2021-01-27 18:40:00+00:00       521  PM2.5  19.00  2021-01-27  18:40:00
     5419 2021-01-27 18:40:00+00:00       521   PM10  39.46  2021-01-27  18:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5423 2021-01-27 18:50:00+00:00       521    PM1  12.44  2021-01-27  18:50:00
     5424 2021-01-27 18:50:00+00:00       521  PM2.5  19.42  2021-01-27  18:50:00
     5425 2021-01-27 18:50:00+00:00       521   PM10  39.08  2021-01-27  18:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5429 2021-01-27 18:59:59+00:00       521    PM1  12.51  2021-01-27  18:59:59
     5430 2021-01-27 18:59:59+00:00       521  PM2.5  20.21  2021-01-27  18:59:59
     5431 2021-01-27 18:59:59+00:00       521   PM10  39.29  2021-01-27  18:59:59,
                          Timestamp  Location   Name  Value        Date      Time
     5435 2021-01-27 19:10:01+00:00       521    PM1  12.93  2021-01-27  19:10:01
     5436 2021-01-27 19:10:01+00:00       521  PM2.5  18.88  2021-01-27  19:10:01
     5437 2021-01-27 19:10:01+00:00       521   PM10  42.27  2021-01-27  19:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     5441 2021-01-27 19:20:00+00:00       521    PM1  13.09  2021-01-27  19:20:00
     5442 2021-01-27 19:20:00+00:00       521  PM2.5  19.17  2021-01-27  19:20:00
     5443 2021-01-27 19:20:00+00:00       521   PM10  46.74  2021-01-27  19:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5447 2021-01-27 19:30:00+00:00       521    PM1  10.54  2021-01-27  19:30:00
     5448 2021-01-27 19:30:00+00:00       521  PM2.5  16.13  2021-01-27  19:30:00
     5449 2021-01-27 19:30:00+00:00       521   PM10  41.47  2021-01-27  19:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5453 2021-01-27 19:40:01+00:00       521    PM1  12.56  2021-01-27  19:40:01
     5454 2021-01-27 19:40:01+00:00       521  PM2.5  17.96  2021-01-27  19:40:01
     5455 2021-01-27 19:40:01+00:00       521   PM10  42.54  2021-01-27  19:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     5459 2021-01-27 19:50:00+00:00       521    PM1  12.08  2021-01-27  19:50:00
     5460 2021-01-27 19:50:00+00:00       521  PM2.5  18.70  2021-01-27  19:50:00
     5461 2021-01-27 19:50:00+00:00       521   PM10  41.60  2021-01-27  19:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5465 2021-01-27 20:00:00+00:00       521    PM1  12.01  2021-01-27  20:00:00
     5466 2021-01-27 20:00:00+00:00       521  PM2.5  16.62  2021-01-27  20:00:00
     5467 2021-01-27 20:00:00+00:00       521   PM10  32.21  2021-01-27  20:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5471 2021-01-27 20:10:00+00:00       521    PM1  11.72  2021-01-27  20:10:00
     5472 2021-01-27 20:10:00+00:00       521  PM2.5  17.33  2021-01-27  20:10:00
     5473 2021-01-27 20:10:00+00:00       521   PM10  35.22  2021-01-27  20:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5477 2021-01-27 20:20:00+00:00       521    PM1  11.39  2021-01-27  20:20:00
     5478 2021-01-27 20:20:00+00:00       521  PM2.5  16.24  2021-01-27  20:20:00
     5479 2021-01-27 20:20:00+00:00       521   PM10  33.05  2021-01-27  20:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5483 2021-01-27 20:30:00+00:00       521    PM1  10.77  2021-01-27  20:30:00
     5484 2021-01-27 20:30:00+00:00       521  PM2.5  16.09  2021-01-27  20:30:00
     5485 2021-01-27 20:30:00+00:00       521   PM10  38.60  2021-01-27  20:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5489 2021-01-27 20:40:00+00:00       521    PM1   9.46  2021-01-27  20:40:00
     5490 2021-01-27 20:40:00+00:00       521  PM2.5  15.57  2021-01-27  20:40:00
     5491 2021-01-27 20:40:00+00:00       521   PM10  29.14  2021-01-27  20:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5495 2021-01-27 20:50:00+00:00       521    PM1   6.62  2021-01-27  20:50:00
     5496 2021-01-27 20:50:00+00:00       521  PM2.5   9.31  2021-01-27  20:50:00
     5497 2021-01-27 20:50:00+00:00       521   PM10  19.19  2021-01-27  20:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5501 2021-01-27 21:00:00+00:00       521    PM1   3.97  2021-01-27  21:00:00
     5502 2021-01-27 21:00:00+00:00       521  PM2.5   4.76  2021-01-27  21:00:00
     5503 2021-01-27 21:00:00+00:00       521   PM10  10.43  2021-01-27  21:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5507 2021-01-27 21:10:00+00:00       521    PM1   3.23  2021-01-27  21:10:00
     5508 2021-01-27 21:10:00+00:00       521  PM2.5   4.34  2021-01-27  21:10:00
     5509 2021-01-27 21:10:00+00:00       521   PM10   9.05  2021-01-27  21:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5512 2021-01-27 21:20:00+00:00       521    PM1   3.60  2021-01-27  21:20:00
     5513 2021-01-27 21:20:00+00:00       521  PM2.5   4.63  2021-01-27  21:20:00
     5514 2021-01-27 21:20:00+00:00       521   PM10   9.41  2021-01-27  21:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5518 2021-01-27 21:30:00+00:00       521    PM1   3.65  2021-01-27  21:30:00
     5519 2021-01-27 21:30:00+00:00       521  PM2.5   4.85  2021-01-27  21:30:00
     5520 2021-01-27 21:30:00+00:00       521   PM10   9.84  2021-01-27  21:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5524 2021-01-27 21:40:00+00:00       521    PM1   3.85  2021-01-27  21:40:00
     5525 2021-01-27 21:40:00+00:00       521  PM2.5   4.97  2021-01-27  21:40:00
     5526 2021-01-27 21:40:00+00:00       521   PM10  10.59  2021-01-27  21:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5530 2021-01-27 21:50:00+00:00       521    PM1   3.31  2021-01-27  21:50:00
     5531 2021-01-27 21:50:00+00:00       521  PM2.5   4.25  2021-01-27  21:50:00
     5532 2021-01-27 21:50:00+00:00       521   PM10   7.83  2021-01-27  21:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5536 2021-01-27 22:00:00+00:00       521    PM1   3.83  2021-01-27  22:00:00
     5537 2021-01-27 22:00:00+00:00       521  PM2.5   4.82  2021-01-27  22:00:00
     5538 2021-01-27 22:00:00+00:00       521   PM10   7.75  2021-01-27  22:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5542 2021-01-27 22:10:00+00:00       521    PM1   3.86  2021-01-27  22:10:00
     5543 2021-01-27 22:10:00+00:00       521  PM2.5   4.89  2021-01-27  22:10:00
     5544 2021-01-27 22:10:00+00:00       521   PM10  11.07  2021-01-27  22:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5548 2021-01-27 22:20:00+00:00       521    PM1   5.30  2021-01-27  22:20:00
     5549 2021-01-27 22:20:00+00:00       521  PM2.5   6.39  2021-01-27  22:20:00
     5550 2021-01-27 22:20:00+00:00       521   PM10   9.78  2021-01-27  22:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5554 2021-01-27 22:30:01+00:00       521    PM1   3.74  2021-01-27  22:30:01
     5555 2021-01-27 22:30:01+00:00       521  PM2.5   4.66  2021-01-27  22:30:01
     5556 2021-01-27 22:30:01+00:00       521   PM10  10.58  2021-01-27  22:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     5560 2021-01-27 22:40:00+00:00       521    PM1   3.19  2021-01-27  22:40:00
     5561 2021-01-27 22:40:00+00:00       521  PM2.5   4.32  2021-01-27  22:40:00
     5562 2021-01-27 22:40:00+00:00       521   PM10   8.57  2021-01-27  22:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5566 2021-01-27 22:50:00+00:00       521    PM1   3.21  2021-01-27  22:50:00
     5567 2021-01-27 22:50:00+00:00       521  PM2.5   3.87  2021-01-27  22:50:00
     5568 2021-01-27 22:50:00+00:00       521   PM10   8.00  2021-01-27  22:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5572 2021-01-27 23:00:00+00:00       521    PM1   3.30  2021-01-27  23:00:00
     5573 2021-01-27 23:00:00+00:00       521  PM2.5   3.96  2021-01-27  23:00:00
     5574 2021-01-27 23:00:00+00:00       521   PM10   7.67  2021-01-27  23:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5578 2021-01-27 23:10:00+00:00       521    PM1   3.18  2021-01-27  23:10:00
     5579 2021-01-27 23:10:00+00:00       521  PM2.5   4.07  2021-01-27  23:10:00
     5580 2021-01-27 23:10:00+00:00       521   PM10   6.60  2021-01-27  23:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5584 2021-01-27 23:20:00+00:00       521    PM1   3.52  2021-01-27  23:20:00
     5585 2021-01-27 23:20:00+00:00       521  PM2.5   4.40  2021-01-27  23:20:00
     5586 2021-01-27 23:20:00+00:00       521   PM10   7.48  2021-01-27  23:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5590 2021-01-27 23:30:00+00:00       521    PM1   3.47  2021-01-27  23:30:00
     5591 2021-01-27 23:30:00+00:00       521  PM2.5   4.21  2021-01-27  23:30:00
     5592 2021-01-27 23:30:00+00:00       521   PM10   7.26  2021-01-27  23:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5596 2021-01-27 23:40:01+00:00       521    PM1   3.37  2021-01-27  23:40:01
     5597 2021-01-27 23:40:01+00:00       521  PM2.5   4.53  2021-01-27  23:40:01
     5598 2021-01-27 23:40:01+00:00       521   PM10   7.21  2021-01-27  23:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     5602 2021-01-27 23:49:59+00:00       521    PM1   2.40  2021-01-27  23:49:59
     5603 2021-01-27 23:49:59+00:00       521  PM2.5   3.24  2021-01-27  23:49:59
     5604 2021-01-27 23:49:59+00:00       521   PM10   6.48  2021-01-27  23:49:59,
                          Timestamp  Location   Name  Value        Date      Time
     5608 2021-01-28 00:00:01+00:00       521    PM1   3.47  2021-01-28  00:00:01
     5609 2021-01-28 00:00:01+00:00       521  PM2.5   4.51  2021-01-28  00:00:01
     5610 2021-01-28 00:00:01+00:00       521   PM10   7.76  2021-01-28  00:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     5614 2021-01-28 00:10:00+00:00       521    PM1   3.51  2021-01-28  00:10:00
     5615 2021-01-28 00:10:00+00:00       521  PM2.5   4.05  2021-01-28  00:10:00
     5616 2021-01-28 00:10:00+00:00       521   PM10   6.75  2021-01-28  00:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5620 2021-01-28 00:20:00+00:00       521    PM1   4.69  2021-01-28  00:20:00
     5621 2021-01-28 00:20:00+00:00       521  PM2.5   5.75  2021-01-28  00:20:00
     5622 2021-01-28 00:20:00+00:00       521   PM10   8.08  2021-01-28  00:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5626 2021-01-28 00:30:00+00:00       521    PM1   3.32  2021-01-28  00:30:00
     5627 2021-01-28 00:30:00+00:00       521  PM2.5   3.51  2021-01-28  00:30:00
     5628 2021-01-28 00:30:00+00:00       521   PM10   6.18  2021-01-28  00:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5632 2021-01-28 00:40:00+00:00       521    PM1   3.42  2021-01-28  00:40:00
     5633 2021-01-28 00:40:00+00:00       521  PM2.5   3.75  2021-01-28  00:40:00
     5634 2021-01-28 00:40:00+00:00       521   PM10   6.29  2021-01-28  00:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5638 2021-01-28 00:50:00+00:00       521    PM1   3.15  2021-01-28  00:50:00
     5639 2021-01-28 00:50:00+00:00       521  PM2.5   3.69  2021-01-28  00:50:00
     5640 2021-01-28 00:50:00+00:00       521   PM10   6.96  2021-01-28  00:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5644 2021-01-28 01:00:00+00:00       521    PM1   2.83  2021-01-28  01:00:00
     5645 2021-01-28 01:00:00+00:00       521  PM2.5   3.27  2021-01-28  01:00:00
     5646 2021-01-28 01:00:00+00:00       521   PM10   6.06  2021-01-28  01:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5650 2021-01-28 01:10:00+00:00       521    PM1   3.33  2021-01-28  01:10:00
     5651 2021-01-28 01:10:00+00:00       521  PM2.5   3.57  2021-01-28  01:10:00
     5652 2021-01-28 01:10:00+00:00       521   PM10   6.07  2021-01-28  01:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5656 2021-01-28 01:20:00+00:00       521    PM1   3.13  2021-01-28  01:20:00
     5657 2021-01-28 01:20:00+00:00       521  PM2.5   3.91  2021-01-28  01:20:00
     5658 2021-01-28 01:20:00+00:00       521   PM10   7.00  2021-01-28  01:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5662 2021-01-28 01:30:00+00:00       521    PM1   3.62  2021-01-28  01:30:00
     5663 2021-01-28 01:30:00+00:00       521  PM2.5   4.24  2021-01-28  01:30:00
     5664 2021-01-28 01:30:00+00:00       521   PM10   6.86  2021-01-28  01:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5668 2021-01-28 01:40:00+00:00       521    PM1   2.44  2021-01-28  01:40:00
     5669 2021-01-28 01:40:00+00:00       521  PM2.5   3.00  2021-01-28  01:40:00
     5670 2021-01-28 01:40:00+00:00       521   PM10   5.32  2021-01-28  01:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5674 2021-01-28 01:50:01+00:00       521    PM1   2.37  2021-01-28  01:50:01
     5675 2021-01-28 01:50:01+00:00       521  PM2.5   2.98  2021-01-28  01:50:01
     5676 2021-01-28 01:50:01+00:00       521   PM10   5.30  2021-01-28  01:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     5680 2021-01-28 02:00:00+00:00       521    PM1   2.56  2021-01-28  02:00:00
     5681 2021-01-28 02:00:00+00:00       521  PM2.5   2.89  2021-01-28  02:00:00
     5682 2021-01-28 02:00:00+00:00       521   PM10   6.83  2021-01-28  02:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5686 2021-01-28 02:10:00+00:00       521    PM1   2.63  2021-01-28  02:10:00
     5687 2021-01-28 02:10:00+00:00       521  PM2.5   3.04  2021-01-28  02:10:00
     5688 2021-01-28 02:10:00+00:00       521   PM10   6.28  2021-01-28  02:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5692 2021-01-28 02:20:00+00:00       521    PM1   2.72  2021-01-28  02:20:00
     5693 2021-01-28 02:20:00+00:00       521  PM2.5   3.38  2021-01-28  02:20:00
     5694 2021-01-28 02:20:00+00:00       521   PM10   6.04  2021-01-28  02:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5698 2021-01-28 02:30:00+00:00       521    PM1   2.68  2021-01-28  02:30:00
     5699 2021-01-28 02:30:00+00:00       521  PM2.5   3.01  2021-01-28  02:30:00
     5700 2021-01-28 02:30:00+00:00       521   PM10   5.47  2021-01-28  02:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5704 2021-01-28 02:40:00+00:00       521    PM1   2.81  2021-01-28  02:40:00
     5705 2021-01-28 02:40:00+00:00       521  PM2.5   3.24  2021-01-28  02:40:00
     5706 2021-01-28 02:40:00+00:00       521   PM10   7.18  2021-01-28  02:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5710 2021-01-28 02:50:00+00:00       521    PM1   2.33  2021-01-28  02:50:00
     5711 2021-01-28 02:50:00+00:00       521  PM2.5   2.87  2021-01-28  02:50:00
     5712 2021-01-28 02:50:00+00:00       521   PM10   5.20  2021-01-28  02:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5716 2021-01-28 03:00:00+00:00       521    PM1   2.56  2021-01-28  03:00:00
     5717 2021-01-28 03:00:00+00:00       521  PM2.5   3.02  2021-01-28  03:00:00
     5718 2021-01-28 03:00:00+00:00       521   PM10   5.37  2021-01-28  03:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5722 2021-01-28 03:10:00+00:00       521    PM1   2.17  2021-01-28  03:10:00
     5723 2021-01-28 03:10:00+00:00       521  PM2.5   2.41  2021-01-28  03:10:00
     5724 2021-01-28 03:10:00+00:00       521   PM10   4.91  2021-01-28  03:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5728 2021-01-28 03:20:00+00:00       521    PM1   2.20  2021-01-28  03:20:00
     5729 2021-01-28 03:20:00+00:00       521  PM2.5   2.51  2021-01-28  03:20:00
     5730 2021-01-28 03:20:00+00:00       521   PM10   4.55  2021-01-28  03:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5734 2021-01-28 03:30:00+00:00       521    PM1   2.05  2021-01-28  03:30:00
     5735 2021-01-28 03:30:00+00:00       521  PM2.5   2.34  2021-01-28  03:30:00
     5736 2021-01-28 03:30:00+00:00       521   PM10   4.42  2021-01-28  03:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5740 2021-01-28 03:40:00+00:00       521    PM1   2.33  2021-01-28  03:40:00
     5741 2021-01-28 03:40:00+00:00       521  PM2.5   2.50  2021-01-28  03:40:00
     5742 2021-01-28 03:40:00+00:00       521   PM10   5.06  2021-01-28  03:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5746 2021-01-28 03:50:00+00:00       521    PM1   2.44  2021-01-28  03:50:00
     5747 2021-01-28 03:50:00+00:00       521  PM2.5   2.90  2021-01-28  03:50:00
     5748 2021-01-28 03:50:00+00:00       521   PM10   5.51  2021-01-28  03:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5751 2021-01-28 04:00:00+00:00       521    PM1   2.68  2021-01-28  04:00:00
     5752 2021-01-28 04:00:00+00:00       521  PM2.5   2.99  2021-01-28  04:00:00
     5753 2021-01-28 04:00:00+00:00       521   PM10   7.62  2021-01-28  04:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5756 2021-01-28 04:10:00+00:00       521    PM1   2.77  2021-01-28  04:10:00
     5757 2021-01-28 04:10:00+00:00       521  PM2.5   3.40  2021-01-28  04:10:00
     5758 2021-01-28 04:10:00+00:00       521   PM10   6.89  2021-01-28  04:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5762 2021-01-28 04:20:01+00:00       521    PM1   2.29  2021-01-28  04:20:01
     5763 2021-01-28 04:20:01+00:00       521  PM2.5   2.65  2021-01-28  04:20:01
     5764 2021-01-28 04:20:01+00:00       521   PM10   5.19  2021-01-28  04:20:01,
                          Timestamp  Location   Name  Value        Date      Time
     5768 2021-01-28 04:30:00+00:00       521    PM1   2.79  2021-01-28  04:30:00
     5769 2021-01-28 04:30:00+00:00       521  PM2.5   3.53  2021-01-28  04:30:00
     5770 2021-01-28 04:30:00+00:00       521   PM10   5.36  2021-01-28  04:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5774 2021-01-28 04:40:00+00:00       521    PM1   2.93  2021-01-28  04:40:00
     5775 2021-01-28 04:40:00+00:00       521  PM2.5   3.34  2021-01-28  04:40:00
     5776 2021-01-28 04:40:00+00:00       521   PM10   7.64  2021-01-28  04:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5780 2021-01-28 04:50:00+00:00       521    PM1   2.97  2021-01-28  04:50:00
     5781 2021-01-28 04:50:00+00:00       521  PM2.5   3.68  2021-01-28  04:50:00
     5782 2021-01-28 04:50:00+00:00       521   PM10   9.32  2021-01-28  04:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5786 2021-01-28 05:00:00+00:00       521    PM1   2.70  2021-01-28  05:00:00
     5787 2021-01-28 05:00:00+00:00       521  PM2.5   3.20  2021-01-28  05:00:00
     5788 2021-01-28 05:00:00+00:00       521   PM10   6.44  2021-01-28  05:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5792 2021-01-28 05:10:00+00:00       521    PM1   2.94  2021-01-28  05:10:00
     5793 2021-01-28 05:10:00+00:00       521  PM2.5   3.11  2021-01-28  05:10:00
     5794 2021-01-28 05:10:00+00:00       521   PM10   6.69  2021-01-28  05:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5798 2021-01-28 05:20:00+00:00       521    PM1   2.57  2021-01-28  05:20:00
     5799 2021-01-28 05:20:00+00:00       521  PM2.5   2.86  2021-01-28  05:20:00
     5800 2021-01-28 05:20:00+00:00       521   PM10   5.53  2021-01-28  05:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5804 2021-01-28 05:30:00+00:00       521    PM1   2.71  2021-01-28  05:30:00
     5805 2021-01-28 05:30:00+00:00       521  PM2.5   3.43  2021-01-28  05:30:00
     5806 2021-01-28 05:30:00+00:00       521   PM10   5.76  2021-01-28  05:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5810 2021-01-28 05:40:00+00:00       521    PM1   2.99  2021-01-28  05:40:00
     5811 2021-01-28 05:40:00+00:00       521  PM2.5   3.75  2021-01-28  05:40:00
     5812 2021-01-28 05:40:00+00:00       521   PM10   6.17  2021-01-28  05:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5816 2021-01-28 05:50:00+00:00       521    PM1   3.15  2021-01-28  05:50:00
     5817 2021-01-28 05:50:00+00:00       521  PM2.5   3.52  2021-01-28  05:50:00
     5818 2021-01-28 05:50:00+00:00       521   PM10   6.07  2021-01-28  05:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5822 2021-01-28 06:00:00+00:00       521    PM1   2.80  2021-01-28  06:00:00
     5823 2021-01-28 06:00:00+00:00       521  PM2.5   3.23  2021-01-28  06:00:00
     5824 2021-01-28 06:00:00+00:00       521   PM10   6.75  2021-01-28  06:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5828 2021-01-28 06:10:00+00:00       521    PM1   2.92  2021-01-28  06:10:00
     5829 2021-01-28 06:10:00+00:00       521  PM2.5   3.22  2021-01-28  06:10:00
     5830 2021-01-28 06:10:00+00:00       521   PM10   6.25  2021-01-28  06:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5834 2021-01-28 06:20:00+00:00       521    PM1   2.69  2021-01-28  06:20:00
     5835 2021-01-28 06:20:00+00:00       521  PM2.5   3.16  2021-01-28  06:20:00
     5836 2021-01-28 06:20:00+00:00       521   PM10   6.13  2021-01-28  06:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5840 2021-01-28 06:30:01+00:00       521    PM1   2.73  2021-01-28  06:30:01
     5841 2021-01-28 06:30:01+00:00       521  PM2.5   3.25  2021-01-28  06:30:01
     5842 2021-01-28 06:30:01+00:00       521   PM10   5.41  2021-01-28  06:30:01,
                          Timestamp  Location   Name  Value        Date      Time
     5846 2021-01-28 06:40:00+00:00       521    PM1   3.70  2021-01-28  06:40:00
     5847 2021-01-28 06:40:00+00:00       521  PM2.5   4.36  2021-01-28  06:40:00
     5848 2021-01-28 06:40:00+00:00       521   PM10   8.09  2021-01-28  06:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5852 2021-01-28 06:50:00+00:00       521    PM1   7.49  2021-01-28  06:50:00
     5853 2021-01-28 06:50:00+00:00       521  PM2.5   8.02  2021-01-28  06:50:00
     5854 2021-01-28 06:50:00+00:00       521   PM10  12.26  2021-01-28  06:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5858 2021-01-28 07:00:00+00:00       521    PM1   9.82  2021-01-28  07:00:00
     5859 2021-01-28 07:00:00+00:00       521  PM2.5  10.55  2021-01-28  07:00:00
     5860 2021-01-28 07:00:00+00:00       521   PM10  13.70  2021-01-28  07:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5864 2021-01-28 07:10:00+00:00       521    PM1   8.94  2021-01-28  07:10:00
     5865 2021-01-28 07:10:00+00:00       521  PM2.5  10.35  2021-01-28  07:10:00
     5866 2021-01-28 07:10:00+00:00       521   PM10  13.73  2021-01-28  07:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5870 2021-01-28 07:20:00+00:00       521    PM1   7.69  2021-01-28  07:20:00
     5871 2021-01-28 07:20:00+00:00       521  PM2.5   8.40  2021-01-28  07:20:00
     5872 2021-01-28 07:20:00+00:00       521   PM10  12.34  2021-01-28  07:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5876 2021-01-28 07:30:00+00:00       521    PM1   6.04  2021-01-28  07:30:00
     5877 2021-01-28 07:30:00+00:00       521  PM2.5   6.78  2021-01-28  07:30:00
     5878 2021-01-28 07:30:00+00:00       521   PM10  12.60  2021-01-28  07:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5882 2021-01-28 07:40:00+00:00       521    PM1   4.71  2021-01-28  07:40:00
     5883 2021-01-28 07:40:00+00:00       521  PM2.5   5.47  2021-01-28  07:40:00
     5884 2021-01-28 07:40:00+00:00       521   PM10   8.63  2021-01-28  07:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5888 2021-01-28 07:50:01+00:00       521    PM1   4.77  2021-01-28  07:50:01
     5889 2021-01-28 07:50:01+00:00       521  PM2.5   5.90  2021-01-28  07:50:01
     5890 2021-01-28 07:50:01+00:00       521   PM10   8.33  2021-01-28  07:50:01,
                          Timestamp  Location   Name  Value        Date      Time
     5894 2021-01-28 08:00:00+00:00       521    PM1   4.67  2021-01-28  08:00:00
     5895 2021-01-28 08:00:00+00:00       521  PM2.5   5.62  2021-01-28  08:00:00
     5896 2021-01-28 08:00:00+00:00       521   PM10   9.35  2021-01-28  08:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5900 2021-01-28 08:10:00+00:00       521    PM1   5.68  2021-01-28  08:10:00
     5901 2021-01-28 08:10:00+00:00       521  PM2.5   6.51  2021-01-28  08:10:00
     5902 2021-01-28 08:10:00+00:00       521   PM10   9.71  2021-01-28  08:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5906 2021-01-28 08:20:00+00:00       521    PM1   7.63  2021-01-28  08:20:00
     5907 2021-01-28 08:20:00+00:00       521  PM2.5   8.38  2021-01-28  08:20:00
     5908 2021-01-28 08:20:00+00:00       521   PM10  11.12  2021-01-28  08:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5912 2021-01-28 08:30:00+00:00       521    PM1   9.80  2021-01-28  08:30:00
     5913 2021-01-28 08:30:00+00:00       521  PM2.5  11.25  2021-01-28  08:30:00
     5914 2021-01-28 08:30:00+00:00       521   PM10  13.87  2021-01-28  08:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5918 2021-01-28 08:40:00+00:00       521    PM1   8.65  2021-01-28  08:40:00
     5919 2021-01-28 08:40:00+00:00       521  PM2.5   9.77  2021-01-28  08:40:00
     5920 2021-01-28 08:40:00+00:00       521   PM10  14.31  2021-01-28  08:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5924 2021-01-28 08:50:00+00:00       521    PM1   7.78  2021-01-28  08:50:00
     5925 2021-01-28 08:50:00+00:00       521  PM2.5   8.69  2021-01-28  08:50:00
     5926 2021-01-28 08:50:00+00:00       521   PM10  12.81  2021-01-28  08:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5930 2021-01-28 09:00:00+00:00       521    PM1   7.40  2021-01-28  09:00:00
     5931 2021-01-28 09:00:00+00:00       521  PM2.5   8.28  2021-01-28  09:00:00
     5932 2021-01-28 09:00:00+00:00       521   PM10  13.35  2021-01-28  09:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5936 2021-01-28 09:10:00+00:00       521    PM1   8.98  2021-01-28  09:10:00
     5937 2021-01-28 09:10:00+00:00       521  PM2.5   9.92  2021-01-28  09:10:00
     5938 2021-01-28 09:10:00+00:00       521   PM10  13.46  2021-01-28  09:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     5942 2021-01-28 09:20:00+00:00       521    PM1   9.63  2021-01-28  09:20:00
     5943 2021-01-28 09:20:00+00:00       521  PM2.5  10.58  2021-01-28  09:20:00
     5944 2021-01-28 09:20:00+00:00       521   PM10  14.73  2021-01-28  09:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5948 2021-01-28 09:30:00+00:00       521    PM1   7.77  2021-01-28  09:30:00
     5949 2021-01-28 09:30:00+00:00       521  PM2.5   9.10  2021-01-28  09:30:00
     5950 2021-01-28 09:30:00+00:00       521   PM10  11.59  2021-01-28  09:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5954 2021-01-28 09:40:00+00:00       521    PM1   8.10  2021-01-28  09:40:00
     5955 2021-01-28 09:40:00+00:00       521  PM2.5   9.34  2021-01-28  09:40:00
     5956 2021-01-28 09:40:00+00:00       521   PM10  12.79  2021-01-28  09:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5960 2021-01-28 09:50:00+00:00       521    PM1   9.01  2021-01-28  09:50:00
     5961 2021-01-28 09:50:00+00:00       521  PM2.5   9.63  2021-01-28  09:50:00
     5962 2021-01-28 09:50:00+00:00       521   PM10  12.62  2021-01-28  09:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     5966 2021-01-28 10:00:00+00:00       521    PM1   7.85  2021-01-28  10:00:00
     5967 2021-01-28 10:00:00+00:00       521  PM2.5   8.55  2021-01-28  10:00:00
     5968 2021-01-28 10:00:00+00:00       521   PM10  11.86  2021-01-28  10:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     5972 2021-01-28 10:10:01+00:00       521    PM1   7.80  2021-01-28  10:10:01
     5973 2021-01-28 10:10:01+00:00       521  PM2.5   8.59  2021-01-28  10:10:01
     5974 2021-01-28 10:10:01+00:00       521   PM10  11.28  2021-01-28  10:10:01,
                          Timestamp  Location   Name  Value        Date      Time
     5978 2021-01-28 10:20:00+00:00       521    PM1   7.52  2021-01-28  10:20:00
     5979 2021-01-28 10:20:00+00:00       521  PM2.5   8.10  2021-01-28  10:20:00
     5980 2021-01-28 10:20:00+00:00       521   PM10  10.90  2021-01-28  10:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     5984 2021-01-28 10:30:00+00:00       521    PM1   6.25  2021-01-28  10:30:00
     5985 2021-01-28 10:30:00+00:00       521  PM2.5   6.49  2021-01-28  10:30:00
     5986 2021-01-28 10:30:00+00:00       521   PM10   9.50  2021-01-28  10:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     5990 2021-01-28 10:40:00+00:00       521    PM1   7.52  2021-01-28  10:40:00
     5991 2021-01-28 10:40:00+00:00       521  PM2.5   7.82  2021-01-28  10:40:00
     5992 2021-01-28 10:40:00+00:00       521   PM10  10.14  2021-01-28  10:40:00,
                          Timestamp  Location   Name  Value        Date      Time
     5996 2021-01-28 10:50:00+00:00       521    PM1   7.49  2021-01-28  10:50:00
     5997 2021-01-28 10:50:00+00:00       521  PM2.5   7.92  2021-01-28  10:50:00
     5998 2021-01-28 10:50:00+00:00       521   PM10   8.85  2021-01-28  10:50:00,
                          Timestamp  Location   Name  Value        Date      Time
     6002 2021-01-28 11:00:00+00:00       521    PM1   5.97  2021-01-28  11:00:00
     6003 2021-01-28 11:00:00+00:00       521  PM2.5   6.18  2021-01-28  11:00:00
     6004 2021-01-28 11:00:00+00:00       521   PM10   7.65  2021-01-28  11:00:00,
                          Timestamp  Location   Name  Value        Date      Time
     6008 2021-01-28 11:10:00+00:00       521    PM1   7.09  2021-01-28  11:10:00
     6009 2021-01-28 11:10:00+00:00       521  PM2.5   7.42  2021-01-28  11:10:00
     6010 2021-01-28 11:10:00+00:00       521   PM10   9.00  2021-01-28  11:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     6014 2021-01-28 11:20:00+00:00       521    PM1   7.19  2021-01-28  11:20:00
     6015 2021-01-28 11:20:00+00:00       521  PM2.5   7.64  2021-01-28  11:20:00
     6016 2021-01-28 11:20:00+00:00       521   PM10   9.20  2021-01-28  11:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     6020 2021-01-28 11:30:00+00:00       521    PM1   8.08  2021-01-28  11:30:00
     6021 2021-01-28 11:30:00+00:00       521  PM2.5   8.40  2021-01-28  11:30:00
     6022 2021-01-28 11:30:00+00:00       521   PM10   9.59  2021-01-28  11:30:00,
                          Timestamp  Location   Name  Value        Date      Time
     6026 2021-01-28 11:40:01+00:00       521    PM1   8.35  2021-01-28  11:40:01
     6027 2021-01-28 11:40:01+00:00       521  PM2.5   8.43  2021-01-28  11:40:01
     6028 2021-01-28 11:40:01+00:00       521   PM10  11.96  2021-01-28  11:40:01,
                          Timestamp  Location   Name  Value        Date      Time
     6032 2021-01-28 11:49:59+00:00       521    PM1   8.88  2021-01-28  11:49:59
     6033 2021-01-28 11:49:59+00:00       521  PM2.5   9.19  2021-01-28  11:49:59
     6034 2021-01-28 11:49:59+00:00       521   PM10  11.61  2021-01-28  11:49:59,
                          Timestamp  Location   Name  Value        Date      Time
     6038 2021-01-28 12:00:01+00:00       521    PM1   9.19  2021-01-28  12:00:01
     6039 2021-01-28 12:00:01+00:00       521  PM2.5   9.57  2021-01-28  12:00:01
     6040 2021-01-28 12:00:01+00:00       521   PM10  11.94  2021-01-28  12:00:01,
                          Timestamp  Location   Name  Value        Date      Time
     6044 2021-01-28 12:10:00+00:00       521    PM1  10.04  2021-01-28  12:10:00
     6045 2021-01-28 12:10:00+00:00       521  PM2.5  10.38  2021-01-28  12:10:00
     6046 2021-01-28 12:10:00+00:00       521   PM10  12.96  2021-01-28  12:10:00,
                          Timestamp  Location   Name  Value        Date      Time
     6050 2021-01-28 12:20:00+00:00       521    PM1  10.84  2021-01-28  12:20:00
     6051 2021-01-28 12:20:00+00:00       521  PM2.5  11.06  2021-01-28  12:20:00
     6052 2021-01-28 12:20:00+00:00       521   PM10  13.07  2021-01-28  12:20:00,
                          Timestamp  Location   Name  Value        Date      Time
     6056 2021-01-28 12:30:00+00:00       521    PM1  11.25  2021-01-28  12:30:00
     6057 2021-01-28 12:30:00+00:00       521  PM2.5  11.39  2021-01-28  12:30:00
     6058 2021-01-28 12:30:00+00:00       521   PM10  13.53  2021-01-28  12:30:00,
     ...]



It looks like the grouping is going according to plan, so now we want to use the info from each grouping to create a row where the different names here are different columns of the row.

```python
for x in date_time_groups.groups:
    current_group = date_time_groups.get_group(x)
    location = current_group['Location'][current_group.index[0]]
    new_row = {'Date': [x[0]], 'Time': [x[1]], 'Location': [location], 'PM10': [np.nan], 'PM2.5': [np.nan],
               'PM1': [np.nan]}
    for row in current_group.values:
        new_row[row[2]] = [row[3]]
    df_521_measurements = pd.concat([df_521_measurements, pd.DataFrame(new_row)])
print(df_521_measurements)
```

              Date      Time Location   PM10  PM2.5   PM1
    0   2021-01-21  13:00:00      521  22.03  10.54  7.29
    0   2021-01-21  13:10:00      521  20.90  10.52  7.02
    0   2021-01-21  13:20:00      521  21.30  10.93  6.66
    0   2021-01-21  13:30:00      521  20.23  11.72  7.67
    0   2021-01-21  13:40:00      521  19.25  11.37  6.90
    ..         ...       ...      ...    ...    ...   ...
    0   2023-05-19  08:20:01      521  19.85  12.33  8.67
    0   2023-05-19  08:30:00      521  29.53  12.79  9.25
    0   2023-05-19  08:40:00      521  21.14  12.51  8.97
    0   2023-05-19  08:50:00      521  19.53  13.44  9.54
    0   2023-05-19  09:00:00      521  26.28  11.33  8.15
    
    [111871 rows x 6 columns]

The measurements do all have an index of 0 now, so we do want to reset the index, as it is already in order.

```python
df_521_measurements = df_521_measurements.reset_index(drop=True)
```

Now we want to visualize a single day at a single location, to see any quick patterns.

```python
df_521_measurements.Date[0]
```




    datetime.date(2021, 1, 21)



```python
df_lcd = df_521_measurements.loc[df_521_measurements['Date'] == pd.to_datetime('2023-02-20')]
print(df_lcd)
```

    C:\Users\micro\AppData\Local\Temp\ipykernel_32672\41694521.py:1: FutureWarning: Comparison of Timestamp with datetime.date is deprecated in order to match the standard library behavior. In a future version these will be considered non-comparable. Use 'ts == pd.Timestamp(date)' or 'ts.date() == date' instead.
      df_lcd = df_521_measurements.loc[df_521_measurements['Date'] == pd.to_datetime('2023-02-20')]

                 Date      Time Location   PM10  PM2.5    PM1
    99235  2023-02-20  00:00:00      521  20.17  15.49  13.95
    99236  2023-02-20  00:10:00      521  20.61  15.38  13.11
    99237  2023-02-20  00:20:00      521  22.29  15.65  12.96
    99238  2023-02-20  00:30:00      521  21.63  15.73  13.35
    99239  2023-02-20  00:40:01      521  23.97  16.24  14.77
    ...           ...       ...      ...    ...    ...    ...
    99373  2023-02-20  23:10:01      521  39.96  29.24  25.05
    99374  2023-02-20  23:20:00      521  52.30  32.50  25.66
    99375  2023-02-20  23:30:00      521  42.31  31.38  24.80
    99376  2023-02-20  23:40:00      521  42.03  30.57  24.89
    99377  2023-02-20  23:50:00      521  39.30  30.58  23.82
    
    [143 rows x 6 columns]

```python
fig, ax = plt.subplots()

fig.canvas.draw()
plt.scatter(df_lcd.index, df_lcd['PM10'], label='PM10', alpha=0.5)
plt.scatter(df_lcd.index, df_lcd['PM2.5'], label='PM2.5', alpha=0.5)
plt.scatter(df_lcd.index, df_lcd['PM2.5'], label='PM1', alpha=0.5)

ax.set_xticks(df_lcd.index[::6])
ax.set_xticklabels(df_lcd['Time'][::6], rotation=90)

fig.suptitle('20-2-2023 Fine dust particles')
ax.set_xlabel('Time')
ax.set_ylabel('Particles (μg/m³)')
plt.legend()
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_50_0.png){: .center-image }

From here we can see that PM1 and PM2.5 are near identical, at least in comparison to PM10. If we look at the actual data we can see there are differences. We can also see that there is some data missing it seems, as after 3:00:00 comes 4:10:00, so somewhere along the lines some 10 minutes went missing it seems. Another thing we can observe is that the increases and decreases of the different particle matters are at similar moments, so there are probably some trends to observe there.

```python
measurement_521_drop_unused
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timestamp</th>
      <th>Location</th>
      <th>Name</th>
      <th>Value</th>
      <th>Date</th>
      <th>Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>54</th>
      <td>2021-01-21 13:00:00+00:00</td>
      <td>521</td>
      <td>PM10</td>
      <td>22.03</td>
      <td>2021-01-21</td>
      <td>13:00:00</td>
    </tr>
    <tr>
      <th>55</th>
      <td>2021-01-21 13:00:00+00:00</td>
      <td>521</td>
      <td>PM2.5</td>
      <td>10.54</td>
      <td>2021-01-21</td>
      <td>13:00:00</td>
    </tr>
    <tr>
      <th>56</th>
      <td>2021-01-21 13:00:00+00:00</td>
      <td>521</td>
      <td>PM1</td>
      <td>7.29</td>
      <td>2021-01-21</td>
      <td>13:00:00</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2021-01-21 13:10:00+00:00</td>
      <td>521</td>
      <td>PM10</td>
      <td>20.90</td>
      <td>2021-01-21</td>
      <td>13:10:00</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2021-01-21 13:10:00+00:00</td>
      <td>521</td>
      <td>PM2.5</td>
      <td>10.52</td>
      <td>2021-01-21</td>
      <td>13:10:00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>670192</th>
      <td>2023-05-19 08:50:00+00:00</td>
      <td>521</td>
      <td>PM2.5</td>
      <td>13.44</td>
      <td>2023-05-19</td>
      <td>08:50:00</td>
    </tr>
    <tr>
      <th>670193</th>
      <td>2023-05-19 08:50:00+00:00</td>
      <td>521</td>
      <td>PM10</td>
      <td>19.53</td>
      <td>2023-05-19</td>
      <td>08:50:00</td>
    </tr>
    <tr>
      <th>670198</th>
      <td>2023-05-19 09:00:00+00:00</td>
      <td>521</td>
      <td>PM1</td>
      <td>8.15</td>
      <td>2023-05-19</td>
      <td>09:00:00</td>
    </tr>
    <tr>
      <th>670199</th>
      <td>2023-05-19 09:00:00+00:00</td>
      <td>521</td>
      <td>PM2.5</td>
      <td>11.33</td>
      <td>2023-05-19</td>
      <td>09:00:00</td>
    </tr>
    <tr>
      <th>670200</th>
      <td>2023-05-19 09:00:00+00:00</td>
      <td>521</td>
      <td>PM10</td>
      <td>26.28</td>
      <td>2023-05-19</td>
      <td>09:00:00</td>
    </tr>
  </tbody>
</table>
<p>335613 rows × 6 columns</p>
</div>


```python
df_521_PM10 = measurement_521_drop_unused.loc[measurement_521_drop_unused['Name'] == "PM10"]
df_521_PM1 = measurement_521_drop_unused.loc[measurement_521_drop_unused['Name'] == "PM1"]
df_521_PM25 = measurement_521_drop_unused.loc[measurement_521_drop_unused['Name'] == "PM2.5"]
```

```python
df_521_PM25 = df_521_PM25.set_index('Timestamp')
df_521_PM1 = df_521_PM1.set_index('Timestamp')
df_521_PM10 = df_521_PM10.set_index('Timestamp')
```

```python
fig, axs = plt.subplots(3, sharex=True, figsize=(15, 10))
fig.suptitle('The different sizes of particles individual values on location 521')
axs[0].plot(df_521_PM10['Value'], 'tab:blue')
axs[0].title.set_text('PM10')
axs[1].plot(df_521_PM25['Value'], 'tab:orange')
axs[1].title.set_text('PM25')
axs[2].plot(df_521_PM1['Value'], 'tab:green')
axs[2].title.set_text('PM1')

plt.tight_layout()

plt.subplots_adjust(hspace=0.75)
plt.xticks(rotation=90)
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_55_0.png){: .center-image }

As we can see even over a longer period of time there are definitely relations between the different sizes of atoms but also individuality. We can also very clearly see two moments where the data was not recorded for a long period of time. In PM10 we can see a little outlier spike at the end of 2021, in PM1 there is an outlier halfway through 2022 and PM2.5 doesn't seem to have any notable outliers.

```python
rolling_mean = df_521_PM10['Value'].rolling(7).mean()
rolling_std = df_521_PM10['Value'].rolling(7).std()
```

```python
plt.figure(figsize=(15, 10))
plt.plot(df_521_PM10['Value'], color="blue", label="Original PM10 Data")
plt.plot(rolling_mean, color="red", label="Rolling Mean PM10")
plt.plot(rolling_std, color="black", label="Rolling Standard Deviation in PM10")
plt.title("PM10 Time Series, Rolling Mean, Standard Deviation")
plt.legend(loc="best")
plt.xticks(rotation=90)
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_58_0.png){: .center-image }

```python
from statsmodels.tsa.stattools import adfuller

adft = adfuller(df_521_PM10['Value'], autolag="AIC")

output_df = pd.DataFrame({"Values": [adft[0], adft[1], adft[2], adft[3], adft[4]['1%'], adft[4]['5%'], adft[4]['10%']],
                          "Metric": ["Test Statistics", "p-value", "No. of lags used", "Number of observations used",
                                     "critical value (1%)", "critical value (5%)", "critical value (10%)"]})
print(output_df)
```

             Values                       Metric
    0 -1.762838e+01              Test Statistics
    1  3.808919e-30                      p-value
    2  6.500000e+01             No. of lags used
    3  1.118050e+05  Number of observations used
    4 -3.430408e+00          critical value (1%)
    5 -2.861566e+00          critical value (5%)
    6 -2.566784e+00         critical value (10%)

The most important part of this is the p-value, or the probability that a particular statistical measure, such as the mean or standard deviation, of an assumed probability distribution will be greater than or equal to (or less than or equal to in some instances) observed results. We have a very small p-value so that is good.

```python
from statsmodels.tsa.seasonal import STL
# plt.figure(figsize=(60, 20))
data = df_521_PM10.resample('10min').mean().ffill()
res = STL(data['Value'], period=144).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

    C:\Users\micro\AppData\Local\Temp\ipykernel_32672\2036538795.py:3: FutureWarning: The default value of numeric_only in DataFrameGroupBy.mean is deprecated. In a future version, numeric_only will default to False. Either specify numeric_only or select only columns which should be valid for the function.
      data = df_521_PM10.resample('10min').mean().ffill()

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_61_1.png){: .center-image }

```python
# plt.figure(figsize=(30, 20))
res2 = STL(data['Value'], period=1008).fit()
res2.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_62_0.png){: .center-image }

```python
# plt.figure(figsize=(30, 20))
res3 = STL(data['Value'], period=30240).fit()
res3.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

Test with weekly seasonality over 1 months

```python
subset1 = data.loc['2021-04-01':'2021-04-30']
res4 = STL(subset1['Value'], period=1008).fit()
res4.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

### Combining all the data
Now we have all the data and we know what a single location looks like and how we need to alter the data so we can use it, we can combine all the data into a single dataset to work from.

```python
location_data = pd.DataFrame(columns=['location_id', 'Lat', 'Lon'])
measurement_full = pd.DataFrame(columns=['Date', 'Time', 'Location', 'PM10', 'PM2.5', 'PM1'])

for dataset in file_list:
    current_df = pd.read_csv(file_path + dataset)

    #get location
    lon = current_df.loc[current_df['Name'] == 'Lon']
    lat = current_df.loc[current_df['Name'] == 'Lat']
    loc_data = {'location_id': [current_df['Location'][0]],
                'Lat': [lat.groupby('Value')['Value'].count().sort_values(ascending=False).head(1).index.values[0]],
                'Lon': [lon.groupby('Value')['Value'].count().sort_values(ascending=False).head(1).index.values[0]]}
    location_data = pd.concat([location_data, pd.DataFrame(loc_data)])

    #get measurements
    df_measurements_data = current_df.loc[:, ['Timestamp', 'Location', 'Name', 'Value']]
    df_measurements_drop_unused = df_measurements_data[
        (df_measurements_data.Name != 'Lat') & (df_measurements_data.Name != 'Lon') & (
                df_measurements_data.Name != 'NO2') & (df_measurements_data.Name != 'UFP')].copy()
    df_measurements_drop_unused['Timestamp'] = pd.to_datetime(df_measurements_drop_unused['Timestamp'])
    df_measurements_drop_unused['Date'] = [d.date() for d in df_measurements_drop_unused['Timestamp']]
    df_measurements_drop_unused['Time'] = [d.time() for d in df_measurements_drop_unused['Timestamp']]
    df_measurements = pd.DataFrame(columns=['Date', 'Time', 'Location', 'PM10', 'PM2.5', 'PM1'])

    date_time_groups = df_measurements_drop_unused.groupby(['Date', 'Time'])
    for x in date_time_groups.groups:
        current_group = date_time_groups.get_group(x)
        location = current_group['Location'][current_group.index[0]]
        new_row = {'Date': [x[0]], 'Time': [x[1]], 'Location': [location], 'PM10': [np.nan], 'PM2.5': [np.nan],
                   'PM1': [np.nan]}
        for row in current_group.values:
            new_row[row[2]] = [row[3]]
        df_measurements = pd.concat([df_measurements, pd.DataFrame(new_row)])

    measurement_full = pd.concat([measurement_full, df_measurements])
    print(dataset)

```

Everything is combined, so let's take a look at whether everything is like we want it to.

It seems like only the index is still a problem, which was to be expected. So if we reset the index, we can save the data into a single file so we can later just load that one to continue preparing and analysing our data.

```python
measurement_full = measurement_full.reset_index(drop=True)
measurement_full.to_csv(
    "C:/Users/Public/Documents/Github/Internship-S5/cdt-a-i-r-zoom-lens-back-end/csv_files/full_measurement.csv",
    index=False)
```

```python
len(measurement_full)
```

We also need to save the location data that we seperated from our raw data.

```python
location_data.to_csv(
    "C:/Users/Public/Documents/Github/Internship-S5/cdt-a-i-r-zoom-lens-back-end/csv_files/location_data.csv",
    index=False)
```

Seeing as we now saved them in a seperate directory than the raw directory, we also want a new file path to work with.

### Analysing the combined data
Now let's get our dataframes again and start analysing them.

```python
df_file_path = "C:/Users/Public/Documents/Github/Internship-S5/cdt-a-i-r-zoom-lens-back-end/csv_files/"
```

```python
loc_df = pd.read_csv(df_file_path + 'location_data.csv')
measurement_df = pd.read_csv(df_file_path + 'full_measurement.csv')
print(loc_df.sample(10))
print(measurement_df.sample(10))
```

        location_id      Lat     Lon
    17          540  51.4797  5.4751
    19          543  51.4390  5.4650
    23          547  51.4686  5.5198
    34          563  51.3565  5.6358
    48          577  51.4904  5.3941
    47          576  51.4584  5.5429
    10          532  51.4815  5.5199
    39          568  51.5305  5.2846
    45          574  51.3072  5.4218
    2           524  52.6559  5.4050
                   Date      Time  Location   PM10  PM2.5    PM1
    2302733  2022-09-02  14:00:00       542  10.06   3.67   3.05
    211651   2022-12-08  08:10:00       522  20.26  15.35  13.07
    5329723  2022-02-25  13:50:00       574  19.92  11.61   7.15
    1030598  2020-08-28  04:20:00       530  15.20   9.73   9.40
    5399156  2019-06-20  05:40:00       575  18.62  10.53   7.87
    5064487  2021-11-13  08:20:01       571  12.31   8.91   7.42
    4703085  2022-10-11  05:20:00       567   8.09   5.56   5.07
    899214   2021-06-22  15:00:00       529  17.98  11.57   9.48
    3611364  2022-07-11  16:20:00       557   5.94   3.42   3.01
    2777516  2022-01-11  23:20:00       546  21.63  18.33  16.00

When we were scraping the data, we noticed that location 575 took much longer than the rest, as it was getting data from 2015. However, let's see how much of that data is still relevant to us.

```python
longest_location = measurement_df[(measurement_df.Location == 575)]
print(longest_location.head(10))
```

                   Date      Time  Location   PM10  PM2.5    PM1
    5390315  2019-04-18  11:50:00       575  19.54  11.46   9.90
    5390316  2019-04-18  12:00:00       575  19.95  10.91   9.86
    5390317  2019-04-18  12:10:00       575  28.21  11.53  10.39
    5390318  2019-04-18  12:20:00       575  14.92  10.62   9.07
    5390319  2019-04-18  12:40:00       575  18.03   9.98   8.84
    5390320  2019-04-18  12:50:00       575  13.04   8.44   6.86
    5390321  2019-04-18  13:20:00       575  14.63   7.20   6.47
    5390322  2019-04-18  13:30:00       575  14.60   6.50   5.67
    5390323  2019-04-18  13:40:00       575  12.77   6.90   5.94
    5390324  2019-04-18  13:50:00       575  16.92   6.87   6.03

It looks like though it started in 2015, the data we need didn't get recorded until April 2019. Let's see what that is like with the other locations, so we know the distribution of when our actual data got collected.

```python
starting_dates = []
for location in measurement_df.Location.unique():
    location_df = measurement_df[(measurement_df.Location == location)]['Date']
    starting_dates.append(location_df.head(1).values[0])

print(starting_dates)
```

    ['2021-01-21', '2020-10-19', '2020-12-02', '2020-12-02', '2020-09-14', '2020-09-14', '2021-01-21', '2020-10-19', '2019-05-16', '2021-03-30', '2020-12-02', '2020-10-19', '2020-12-02', '2020-10-19', '2020-09-14', '2020-10-19', '2020-12-02', '2020-10-19', '2020-12-02', '2020-09-14', '2020-09-14', '2020-12-02', '2020-10-19', '2021-01-20', '2020-11-09', '2020-10-19', '2020-12-02', '2021-01-21', '2020-12-02', '2021-01-22', '2020-10-19', '2020-10-19', '2021-01-21', '2020-09-14', '2021-01-22', '2020-09-14', '2021-01-21', '2020-09-14', '2021-01-21', '2021-01-21', '2021-05-01', '2021-03-30', '2021-03-30', '2021-03-26', '2021-05-01', '2019-04-18', '2019-04-18', '2021-05-01', '2021-05-01']

Now we have the first date of relevant data of each location, let's see how it is distributed.

```python
from collections import Counter, OrderedDict

c = Counter(starting_dates)
ordered_data = OrderedDict(sorted(c.items(), key=lambda x: datetime.strptime(x[0], '%Y-%m-%d'), reverse=False))

plt.bar(ordered_data.keys(), ordered_data.values())
plt.suptitle("Amount of locations that started recording relevant data on that date")
plt.xticks(rotation=90)
plt.xlabel("Start date")
plt.ylabel("Amount of locations")
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_83_0.png){: .center-image }

As we can see, there were two locations that started recording in April 2019, which were the first. So the earliest records are from April 18th 2019. Most locations started recording at the end of 2020, somewhere between September 14th and December 2nd. There were also still quite some locations that started recording at the start of 2021, and the latest a location started recording was May 2021. Something of note here, is that this means that we have 2 sensors that were placed before covid, while the rest was placed during. This means that outliers in observations don't have a 'base level' from before covid to compare it with. This needs to be taken into account when analysing the results.

Something else that we need to verify, is whether everything is in the same unit. We know at location 521 it was, but we need to verify whether this is the case with all the locations.

```python
units = []
for file in file_list:
    df_temp = pd.read_csv(file_path + file)
    unique_units = df_temp.groupby('Name')['Unit'].unique()
    units.append(unique_units)
```

```python
unit_df = pd.DataFrame()
for unit in range(len(units)):
    for row in range(len(units[unit])):
        units[unit][row] = units[unit][row][0]
    unit_df = pd.concat([unit_df, units[unit].to_frame().T], ignore_index=True)

unique_unit = unit_df.drop_duplicates()
print(unique_unit)
```

    Name Lat Lon        NO2    PM1   PM10  PM2.5             UFP
    0      °   °  μg/m³\r\n  μg/m³  μg/m³  μg/m³  counts/cm³\r\n
    6      °   °  μg/m³\r\n  μg/m³  μg/m³  μg/m³             NaN

It looks like the only place where there is a difference in the unit values, is in a location where the UFP unit is NaN. However, seeing as we are not doing anything with the UFP unit, this doesn't matter, so now we have verified that the same unit of measurement is used in every single location. Now we have verified everything we needed, we can take a definitive look at the data quality.

### Data Quality
Usually you would check the data quality before you prepare the data, so you can adjust accordingly. However, in our case preparing the data and collecting it was intertwined, adjusting the way of collecting so there was less need for preparing it. That is why the data quality is checked after the initial preparation of the data, as the data is not yet completely prepared, but enough that we can work with it.

`Validity - Check whether there is always a number when we expect it etc`
`Accuracy - We cannot definitevely say it is accurate, for example the location keeps jumping so it is prone to mistakes, but we don't need 100% accurate + this is why we trust this source and sample a few from samenmeten and explain some differences`
`Consistency - The different units are the same`
`Completeness - Missing data, how much do we have etc`
`Uniqueness - It is not unique, as samenmeten enzo het ook heeft, but this is the difference between those things`
`Timeliness - Check the time it takes to make a request to the api for 1 location for right now`

#### Validity
Let's start by looking at info to see whether the type matches.

```python
measurement_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5739479 entries, 0 to 5739478
    Data columns (total 6 columns):
     #   Column    Dtype  
    ---  ------    -----  
     0   Date      object 
     1   Time      object 
     2   Location  int64  
     3   PM10      float64
     4   PM2.5     float64
     5   PM1       float64
    dtypes: float64(3), int64(1), object(2)
    memory usage: 262.7+ MB

It looks like the type is matching with what we are expecting, the date and time are objects and location, location is an int as it an ID and PM10, PM2.5 and PM1 are all floats as they are measurements. However, let's check whether they are all positive, as a negative amount of fine dust does not exist.

```python
measurement_df.describe()
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Location</th>
      <th>PM10</th>
      <th>PM2.5</th>
      <th>PM1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>5.739479e+06</td>
      <td>5.735059e+06</td>
      <td>5.739478e+06</td>
      <td>5.739479e+06</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>5.485570e+02</td>
      <td>1.835883e+01</td>
      <td>1.273283e+01</td>
      <td>1.031460e+01</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.692974e+01</td>
      <td>1.443371e+01</td>
      <td>1.396131e+01</td>
      <td>1.273105e+01</td>
    </tr>
    <tr>
      <th>min</th>
      <td>5.210000e+02</td>
      <td>-0.000000e+00</td>
      <td>-3.200000e+00</td>
      <td>-3.590000e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>5.330000e+02</td>
      <td>9.990000e+00</td>
      <td>5.500000e+00</td>
      <td>4.120000e+00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5.470000e+02</td>
      <td>1.518000e+01</td>
      <td>9.330000e+00</td>
      <td>7.140000e+00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>5.640000e+02</td>
      <td>2.360000e+01</td>
      <td>1.652000e+01</td>
      <td>1.352000e+01</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.770000e+02</td>
      <td>1.196890e+03</td>
      <td>1.358270e+03</td>
      <td>1.355330e+03</td>
    </tr>
  </tbody>
</table>
</div>


As we can see, there are some negative values. Not a lot, as they do not reach past the 25% quartile, however they do exist. Let's see how often they occur.

```python
negative_percentages = pd.DataFrame()

for location in measurement_df.Location.unique():
    loc_data = measurement_df[(measurement_df.Location == location)]
    neg_data = loc_data[(loc_data.PM10 < 0) | (loc_data['PM2.5'] < 0) | (loc_data.PM1 < 0)]
    neg_df_data = {'loc_id': [location], 'neg_amounts': [len(neg_data)], 'total_amounts': [len(loc_data)],
                   'percentage_negative': [(len(neg_data) / len(loc_data)) * 100]}
    negative_percentages = pd.concat([negative_percentages, pd.DataFrame(neg_df_data)])

print(negative_percentages.sort_values('percentage_negative', ascending=False))
```

       loc_id  neg_amounts  total_amounts  percentage_negative
    0     566         5662         120626             4.693847
    0     537         3388         130891             2.588413
    0     573          805          43670             1.843371
    0     544         2304         127384             1.808704
    0     553         2061         115215             1.788830
    0     560         1689         118812             1.421574
    0     572          672          74668             0.899984
    0     562         1037         130176             0.796614
    0     540          856         128359             0.666880
    0     551          691         118343             0.583896
    0     536          640         130430             0.490685
    0     522          546         117745             0.463714
    0     526          633         139222             0.454670
    0     527          546         134782             0.405099
    0     568          423         109085             0.387771
    0     558          425         118283             0.359308
    0     546          424         121509             0.348945
    0     529          369         113568             0.324915
    0     569          327         104493             0.312940
    0     524          363         123710             0.293428
    0     570          287          98287             0.292002
    0     530          528         183580             0.287613
    0     535          291         106338             0.273656
    0     575          338         147377             0.229344
    0     574          251         136859             0.183400
    0     545          171         125742             0.135993
    0     525          162         121321             0.133530
    0     556           98         112554             0.087069
    0     571           73          95844             0.076165
    0     533          100         132893             0.075249
    0     549           61         116901             0.052181
    0     528           54         116834             0.046219
    0     534           55         124645             0.044125
    0     543           58         133510             0.043442
    0     567           46         110198             0.041743
    0     576            8         102129             0.007833
    0     577            2          99658             0.002007
    0     547            1         112618             0.000888
    0     565            0         118270             0.000000
    0     564            0         125884             0.000000
    0     563            0         114562             0.000000
    0     561            0         114681             0.000000
    0     557            0         116113             0.000000
    0     554            0         116303             0.000000
    0     542            0         125188             0.000000
    0     539            0         125269             0.000000
    0     532            0          78550             0.000000
    0     531            0          94529             0.000000
    0     521            0         111871             0.000000

```python
plt.hist(negative_percentages.percentage_negative)
plt.suptitle("Distribution of percentage of negative values")
plt.xlabel("% of negative values")
plt.ylabel("Distribution")
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_96_0.png){: .center-image }

As we can see there are a few negative values, but most location have less than 1% or even no negative values. There are a few that are a bit higher than 1% and one is even just over 5%. It is not extremely concerning however it is a little bit damaging to the validity. However, the combination of correct types and only a few negative values means the *Validity of the data is decent, it is quite good but there is the issue with negative values.

#### Accuracy
We cannot definitively say the data is accurate. It is not something that you can classify, such as whether an animal is a dog. It is a measurement and measurements can be wrong and especially with something like fine dust, where the difference between the sensor and a meter away from the sensor can be very different, it is constantly changing based on the wind etc, we can only determine how much we trust we put in the idea that the data is accurate. For this we will look at a few aspects:
- Who is collecting it
- Who is using it
- Samenmeten

The data is being collected by ODZOB, the Omgevingsdienst Zuidoost-Brabant (Environment Service South-East Brabant). They work on executing the ecology and construction tasks mandated by law such as preventing noise disturbance, limiting health risks and tackling soil pollution {SOURCE, https://odzob.nl/expertises}. This means that the results of this organisation are trusted to be accurate enough for tasks mandated by law. This is also shown as they work with multiple municipalities. It is used to inform those who are making the local laws. Samenmeten, the portal of the RIVM, which is the national institute for public health and the environment, is also sharing the data of the sensors on their dataportal. So if local lawmakers are using the data, and the national institutes are sharing the data, and we know it is collected by experts, we can say that the *Accuracy* is acceptable.



#### Consistency
We know from checking the units just now, that the units used are consistent. So there is definetely a shared understanding of definition between the different locations. So we know the *Consistency* of the data is good.

#### Completeness
We know from the .info() that there aren't any NaN values. However, this doesn't mean that there isn't any missing data. It just means that all the times that data was sent through, all the data was present. However when we were previously visualizing the single location data, we saw that there was a jump of 10 minutes, so that means that somewhere along the lines a measurement wasn't collected once. Which isn't that bad, however this was only once on the last collected day on one location. So let's see how much missing timestamps there are overall.
(Is there any missing data, how much do we have etc)

```python
measurement_df.head()
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Time</th>
      <th>Location</th>
      <th>PM10</th>
      <th>PM2.5</th>
      <th>PM1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-01-21</td>
      <td>13:00:00</td>
      <td>521</td>
      <td>22.03</td>
      <td>10.54</td>
      <td>7.29</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-01-21</td>
      <td>13:10:00</td>
      <td>521</td>
      <td>20.90</td>
      <td>10.52</td>
      <td>7.02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-01-21</td>
      <td>13:20:00</td>
      <td>521</td>
      <td>21.30</td>
      <td>10.93</td>
      <td>6.66</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-01-21</td>
      <td>13:30:00</td>
      <td>521</td>
      <td>20.23</td>
      <td>11.72</td>
      <td>7.67</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-01-21</td>
      <td>13:40:00</td>
      <td>521</td>
      <td>19.25</td>
      <td>11.37</td>
      <td>6.90</td>
    </tr>
  </tbody>
</table>
</div>


```python
import time

sets_of_10 = pd.DataFrame()

fmt = '%Y-%m-%d %H:%M:%S'
for location in measurement_df.Location.unique():
    loc_data = measurement_df[(measurement_df.Location == location)]
    d1 = datetime.strptime(loc_data.head(1).Date.values[0] + ' ' + loc_data.head(1).Time.values[0], fmt)
    d2 = datetime.strptime(loc_data.tail(1).Date.values[0] + ' ' + loc_data.tail(1).Time.values[0], fmt)

    d1_ts = time.mktime(d1.timetuple())
    d2_ts = time.mktime(d2.timetuple())

    supposed_sets = int(int(d2_ts - d1_ts) / 600)
    sets_data = {'loc_id': [location], 'supposed_sets': [supposed_sets], 'actual_sets': [len(loc_data)],
                 'percentage_missing': [100 - (
                         (len(loc_data) / supposed_sets) * 100)]}
    sets_of_10 = pd.concat([sets_of_10, pd.DataFrame(sets_data)])

print(sets_of_10)
```

       loc_id  supposed_sets  actual_sets  percentage_missing
    0     521         122082       111871            8.364050
    0     522         130448       117745            9.737980
    0     524         129296       123710            4.320319
    0     525         129296       121321            6.168018
    0     526         140660       139222            1.022323
    0     527         140661       134782            4.179552
    0     528         122085       116834            4.301102
    0     529         135641       113568           16.273103
    0     530         210787       183580           12.907342
    0     531         112308        94529           15.830573
    0     532          94121        78550           16.543598
    0     533         135639       132893            2.024491
    0     534         129295       124645            3.596427
    0     535         135640       106338           21.602772
    0     536         140660       130430            7.272857
    0     537         135641       130891            3.501891
    0     539         129296       125269            3.114559
    0     540         135641       128359            5.368583
    0     542         129294       125188            3.175708
    0     543         140659       133510            5.082504
    0     544         140659       127384            9.437718
    0     545         129295       125742            2.747979
    0     546         135640       121509           10.418018
    0     547         122214       112618            7.851801
    0     549         131090       116901           10.823861
    0     551         135635       118343           12.748922
    0     553         129281       115215           10.880176
    0     554         122083       116303            4.734484
    0     556         129296       112554           12.948583
    0     557         121926       116113            4.767646
    0     558         135630       118283           12.789943
    0     560         135641       118812           12.407016
    0     561         122084       114681            6.063858
    0     562         140661       130176            7.454092
    0     563         121923       114562            6.037417
    0     564         140661       125884           10.505400
    0     565         122085       118270            3.124872
    0     566         140662       120626           14.244074
    0     567         122086       110198            9.737398
    0     568         122086       109085           10.649051
    0     569         107770       104493            3.040735
    0     570         112312        98287           12.487535
    0     571         112312        95844           14.662725
    0     572         112863        74668           33.841915
    0     573          49251        43670           11.331750
    0     574         214835       136859           36.295762
    0     575         214838       147377           31.400869
    0     576         107775       102129            5.238692
    0     577         107777        99658            7.533147

As we can see, there is definitely some missing timestamps. Now let's see how much.

```python
plt.hist(sets_of_10.percentage_missing, bins=10)
plt.suptitle('Distribution of Percentage of Missing Timestamps')
plt.xlabel('Percentage missing')
plt.ylabel('Amount of locations')
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_104_0.png){: .center-image }

As we can see, most locations have around 10% of missing data or under. There are a few with more, however for now it looks like, with years of data every there is usually a max of 20% data loss over 45 locations. The 4 remaining locations, do have a larger amount of missing data, between 20 to 40%. However, it might be because at the start the data collection didn't go as well. Let's see which locations have this much missing data and whether they are the locations that have been around the longest.

```python
highest_missing = sets_of_10.sort_values('percentage_missing', ascending=False)[:4]
for location in highest_missing.loc_id:
    loc_data = measurement_df[(measurement_df.Location == location)]
print(loc_data.head(1))
```

                   Date      Time  Location  PM10  PM2.5   PM1
    1593250  2020-10-19  10:29:55       535  1.96  -0.79 -0.79

#### Uniqueness
The data is not very unique. There are similar versions of fine dust being measured around Eindhoven, as can be seen on the samenmeten platform of the RIVD. However, there also aren't a lot of datasets like it, with the exception of the wide scale similar measurements that samenmeten shows. This, in combination with the difference in measurement techniques makes the *Uniqueness* of the data moderate.

#### Timeliness
It took quite a while to get the data from the API, so someone who wants to collect the data from scratch the timeliness will be bad. However, now that we have all the data, the only factor is how quick the API can get a small amount of data, namely the current data.

```python
import requests as request
start_time = time.time()
res = request.get("https://api.dustmonitoring.nl/v1/project(30)/observations?$expand=data&$filter=location%20eq%20521")
data = res.json()
print("--- %s seconds ---" % (time.time() - start_time))
```

    --- 0.25713229179382324 seconds ---

It looks like it doesn't take long to get the data from the API if you aren't doing it in bulk. This is fortunate as it means that the *Timeliness* of the data (after it is already collected) is excellent.

### Compare with report
Comparing with ILM2 report

Subdivide known location IDS into airport, city and outdoor.

```python
airport_loc = ['I02', 'I14', 'I25']
outdoors_loc = ['I18', 'I45', 'I42', 'I43', 'I33', 'I03', 'I46', 'I47', 'I58', 'I13', 'I56', 'I10', 'I05', 'I41', 'I16', 'I44'] # 4 extra? I55 I54
city_loc = ['I51', 'I49', 'I52', 'I48', 'I04', 'I32', 'I37', 'I23', 'I07', 'I22', 'I28', 'I40', 'I08', 'I30', 'I19', 'I36', 'I39', 'I29', 'I12', 'I09', 'I11', 'I24'] # 'I57'
```

```python
divided_loc_df = pd.DataFrame()

for airport in airport_loc:
    loc = airboxes[airboxes['Name'] == airport]
    dict_loc = {'ID': loc['ID'].values, 'Name': loc['Name'].values, 'Group': ['Airport']}
    divided_loc_df = pd.concat([divided_loc_df, pd.DataFrame(dict_loc)])

for outdoor in outdoors_loc:
    loc = airboxes[airboxes['Name'] == outdoor]
    dict_loc = {'ID': loc['ID'].values, 'Name': loc['Name'].values, 'Group': ['Outdoors']}
    divided_loc_df = pd.concat([divided_loc_df, pd.DataFrame(dict_loc)])

for city in city_loc:
    loc = airboxes[airboxes['Name'] == city]
    dict_loc = {'ID': loc['ID'].values, 'Name': loc['Name'].values, 'Group': ['City']}
    divided_loc_df = pd.concat([divided_loc_df, pd.DataFrame(dict_loc)])

print(divided_loc_df)
```

        ID Name     Group
    0  521  I02   Airport
    0  536  I14   Airport
    0  525  I25   Airport
    0  532  I18  Outdoors
    0  564  I45  Outdoors
    0  561  I42  Outdoors
    0  562  I43  Outdoors
    0  558  I33  Outdoors
    0  542  I03  Outdoors
    0  565  I46  Outdoors
    0  566  I47  Outdoors
    0  577  I58  Outdoors
    0  542  I13  Outdoors
    0  575  I56  Outdoors
    0  554  I10  Outdoors
    0  553  I05  Outdoors
    0  560  I41  Outdoors
    0  556  I16  Outdoors
    0  563  I44  Outdoors
    0  570  I51      City
    0  568  I49      City
    0  571  I52      City
    0  567  I48      City
    0  543  I04      City
    0  540  I32      City
    0  528  I37      City
    0  524  I23      City
    0  545  I07      City
    0  549  I22      City
    0  526  I28      City
    0  530  I40      City
    0  532  I08      City
    0  551  I30      City
    0  547  I19      City
    0  527  I36      City
    0  529  I39      City
    0  539  I29      City
    0  535  I12      City
    0  533  I09      City
    0  534  I11      City
    0  537  I24      City

Expected: 99% uptime.

```python
import time

df_2021_observation = pd.DataFrame()
df_2021 = pd.DataFrame()

fmt = '%Y-%m-%d %H:%M:%S'
for location in measurement_df.Location.unique():
    loc_data = measurement_df[(measurement_df.Location == location)]
    d1 = datetime.strptime(loc_data.head(1).Date.values[0] + ' ' + loc_data.head(1).Time.values[0], fmt)
    if d1.date().year < 2021:
        d1 = datetime.strptime('2021-01-01 00:00:00', fmt)
    # d2 = datetime.strptime(loc_data.tail(1).Date.values[0] + ' ' + loc_data.tail(1).Time.values[0], fmt)
    d2 = datetime.strptime('2021-12-31 23:59:59', fmt)

    d1_ts = time.mktime(d1.timetuple())
    d2_ts = time.mktime(d2.timetuple())

    mask = (loc_data['Date'] > str(d1.date())) & (loc_data['Date'] <= str(d2.date()))
    loc_2021_df = loc_data.loc[mask]
    df_2021 = pd.concat([df_2021, loc_2021_df])

    supposed_sets = int(int(d2_ts - d1_ts) / 600)
    sets_data = {'loc_id': [location], 'supposed_sets': [supposed_sets], 'actual_sets': [len(loc_2021_df)],
                 'percentage_missing': [100 - (
                         (len(loc_2021_df) / supposed_sets) * 100)]}
    df_2021_observation = pd.concat([df_2021_observation, pd.DataFrame(sets_data)])

print(df_2021_observation)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[6], line 7
          4 df_2021 = pd.DataFrame()
          6 fmt = '%Y-%m-%d %H:%M:%S'
    ----> 7 for location in measurement_df.Location.unique():
          8     loc_data = measurement_df[(measurement_df.Location == location)]
          9     d1 = datetime.strptime(loc_data.head(1).Date.values[0] + ' ' + loc_data.head(1).Time.values[0], fmt)
    

    NameError: name 'measurement_df' is not defined


```python
plt.hist(df_2021_observation.percentage_missing, bins=50)
plt.suptitle('Distribution of Percentage of Missing Timestamps in 2021')
plt.xlabel('Percentage missing')
plt.ylabel('Amount of locations')
plt.show()
```

![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_116_0.png){: .center-image }

Result: about half of the locations come close to this 99% uptime.

Yearly average per amount

```python
ilm2_df = df_2021[(df_2021.Location == 521)]
print('For ILM2:')
print('PM10 year mean: ' + str(ilm2_df['PM10'].mean()))
print('PM2.5 year mean: ' + str(ilm2_df['PM2.5'].mean()))
print('PM1 year mean: ' + str(ilm2_df['PM1'].mean()))

print('\nOverall:')
print('PM10 year mean: ' + str(df_2021['PM10'].mean()))
print('PM2.5 year mean: ' + str(df_2021['PM2.5'].mean()))
print('PM1 year mean: ' + str(df_2021['PM1'].mean()))
```

    For ILM2:
    PM10 year mean: 20.534862313665986
    PM2.5 year mean: 14.855719226992772
    PM1 year mean: 12.831332009283232
    
    Overall:
    PM10 year mean: 18.318628310984085
    PM2.5 year mean: 12.777047338836285
    PM1 year mean: 10.463851545963406

Expected ILM:
PM10: 24,5
PM2.5: 18,7

Result ILM:
PM10: 20,5
PM2.5: 14,8

Expected Total:
PM10: 11,5
PM2.5: 13,9
PM1: 19,3

Result Total:
PM10: 18,3
PM2.5: 12,8
PM1: 10.5

```python
df_2021
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Time</th>
      <th>Location</th>
      <th>PM10</th>
      <th>PM2.5</th>
      <th>PM1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>66</th>
      <td>2021-01-22</td>
      <td>00:00:00</td>
      <td>521</td>
      <td>6.42</td>
      <td>3.42</td>
      <td>2.77</td>
    </tr>
    <tr>
      <th>67</th>
      <td>2021-01-22</td>
      <td>00:10:01</td>
      <td>521</td>
      <td>6.53</td>
      <td>2.67</td>
      <td>2.15</td>
    </tr>
    <tr>
      <th>68</th>
      <td>2021-01-22</td>
      <td>00:20:00</td>
      <td>521</td>
      <td>7.15</td>
      <td>2.93</td>
      <td>2.03</td>
    </tr>
    <tr>
      <th>69</th>
      <td>2021-01-22</td>
      <td>00:30:00</td>
      <td>521</td>
      <td>6.84</td>
      <td>2.52</td>
      <td>1.95</td>
    </tr>
    <tr>
      <th>70</th>
      <td>2021-01-22</td>
      <td>00:40:00</td>
      <td>521</td>
      <td>6.32</td>
      <td>2.60</td>
      <td>2.06</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5667539</th>
      <td>2021-12-31</td>
      <td>23:10:04</td>
      <td>577</td>
      <td>23.53</td>
      <td>16.54</td>
      <td>11.50</td>
    </tr>
    <tr>
      <th>5667540</th>
      <td>2021-12-31</td>
      <td>23:20:04</td>
      <td>577</td>
      <td>26.89</td>
      <td>20.32</td>
      <td>14.93</td>
    </tr>
    <tr>
      <th>5667541</th>
      <td>2021-12-31</td>
      <td>23:30:04</td>
      <td>577</td>
      <td>24.43</td>
      <td>18.48</td>
      <td>12.45</td>
    </tr>
    <tr>
      <th>5667542</th>
      <td>2021-12-31</td>
      <td>23:40:04</td>
      <td>577</td>
      <td>25.76</td>
      <td>20.31</td>
      <td>15.27</td>
    </tr>
    <tr>
      <th>5667543</th>
      <td>2021-12-31</td>
      <td>23:50:04</td>
      <td>577</td>
      <td>32.12</td>
      <td>24.04</td>
      <td>17.81</td>
    </tr>
  </tbody>
</table>
<p>2194942 rows × 6 columns</p>
</div>


```python
divided_loc_df = divided_loc_df.reset_index(drop=True)
print(divided_loc_df)
```

         ID Name     Group
    0   521  I02   Airport
    1   536  I14   Airport
    2   525  I25   Airport
    3   532  I18  Outdoors
    4   564  I45  Outdoors
    5   561  I42  Outdoors
    6   562  I43  Outdoors
    7   558  I33  Outdoors
    8   542  I03  Outdoors
    9   565  I46  Outdoors
    10  566  I47  Outdoors
    11  577  I58  Outdoors
    12  542  I13  Outdoors
    13  575  I56  Outdoors
    14  554  I10  Outdoors
    15  553  I05  Outdoors
    16  560  I41  Outdoors
    17  556  I16  Outdoors
    18  563  I44  Outdoors
    19  570  I51      City
    20  568  I49      City
    21  571  I52      City
    22  567  I48      City
    23  543  I04      City
    24  540  I32      City
    25  528  I37      City
    26  524  I23      City
    27  545  I07      City
    28  549  I22      City
    29  526  I28      City
    30  530  I40      City
    31  532  I08      City
    32  551  I30      City
    33  547  I19      City
    34  527  I36      City
    35  529  I39      City
    36  539  I29      City
    37  535  I12      City
    38  533  I09      City
    39  534  I11      City
    40  537  I24      City

Monthly average per group.

```python
groups_2021 = pd.DataFrame()
for location in divided_loc_df['ID']:
    loc_specific = df_2021[(df_2021.Location == location)]
    # if divided_loc_df[(divided_loc_df.ID == location)].Group.values == ['Airport']:
    #     loc_specific['Group'] = 'Airport'
    print(str(location) + ' ' + divided_loc_df[(divided_loc_df.ID == location)].Group.values)
    # if divided_loc_df[(divided_loc_df.ID == location)].Group.values == ['Outdoors']:
    #     loc_specific['Group'] = 'Outdoors'
    # if divided_loc_df[(divided_loc_df.ID == location)].Group.values == ['City']:
    #     loc_specific['Group'] = 'City'
    # groups_2021 = pd.concat([groups_2021, loc_specific])
    # groups_2021_airport = pd.concat(groups_2021_airport, df_2021[(df_2021.Location == location) and (divided_loc_df[location]['Group'] == 'Airport')])
    # groups_2021_outdoors = df_2021[(df_2021.Location == location.ID) and (location['Group'] == 'Outdoors')]
    # groups_2021_city = df_2021[(df_2021.Location == location.ID) and (location['Group'] == 'City')]

groups_2021
# print(groups_2021_airport)

# print('For ILM2:')
# print('PM10 year mean: ' + str(ilm2_df['PM10'].mean()))
# print('PM2.5 year mean: ' + str(ilm2_df['PM2.5'].mean()))
# print('PM1 year mean: ' + str(ilm2_df['PM1'].mean()))
#
# print('\nOverall:')
# print('PM10 year mean: ' + str(df_2021['PM10'].mean()))
# print('PM2.5 year mean: ' + str(df_2021['PM2.5'].mean()))
# print('PM1 year mean: ' + str(df_2021['PM1'].mean()))
```

    ['521 Airport']
    ['536 Airport']
    ['525 Airport']
    ['532 Outdoors' '532 City']
    ['564 Outdoors']
    ['561 Outdoors']
    ['562 Outdoors']
    ['558 Outdoors']
    ['542 Outdoors' '542 Outdoors']
    ['565 Outdoors']
    ['566 Outdoors']
    ['577 Outdoors']
    ['542 Outdoors' '542 Outdoors']
    ['575 Outdoors']
    ['554 Outdoors']
    ['553 Outdoors']
    ['560 Outdoors']
    ['556 Outdoors']
    ['563 Outdoors']
    ['570 City']
    ['568 City']
    ['571 City']
    ['567 City']
    ['543 City']
    ['540 City']
    ['528 City']
    ['524 City']
    ['545 City']
    ['549 City']
    ['526 City']
    ['530 City']
    ['532 Outdoors' '532 City']
    ['551 City']
    ['547 City']
    ['527 City']
    ['529 City']
    ['539 City']
    ['535 City']
    ['533 City']
    ['534 City']
    ['537 City']



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>


All airbox locations are unique

```python
print(len(airboxes.ID))
print(len(airboxes.ID.unique()))
print(len(airboxes.Name.unique()))
print(len(airboxes.Description.unique()))
```

    42
    40
    42
    40

There are two IDs that are used twice, and have a 'unique' name and the lon lat combo also doesn't match.

---
Now with more insights:

- 75% of relevant period needs to be measured
- background noise is measured by each hour the lowest average over the entire ILM2
- 'Weekgang' where you show an average week, measures its averages per hour.

Things that are odd based on previous ideas:
- Negative values should be automatically excluded
- There should be about 35 days of no data max, in 1 location, there are only 8 locations that have 1+ day of no data.

'Claims' that we want to verify/compare:
- The average over a week of PM2.5, PM10 and NO2 with their 25/75 percentile
- (maybe: Wind direction plot measured based on eindhoven airport weather station)
- Negative values are only mentioned on NO2, is this correct
- 8 locations have 1+ day of no data, max of 35 days.
- Fig 10, profile of the month of june in 10 min measurements
- ILM037 has an average of 20.9 ug/m3 PM10

```python
df_521_NO2 = measurement_521_data[
    (measurement_521_data.Name == 'NO2')].copy()

df_521_NO2['Timestamp'] = pd.to_datetime(df_521_NO2['Timestamp'])
df_521_NO2['Date'] = [d.date() for d in df_521_NO2['Timestamp']]
df_521_NO2['Time'] = [d.time() for d in df_521_NO2['Timestamp']]
df_521_NO2 = df_521_NO2.set_index('Timestamp')

df_521_NO2['Weekday'] = df_521_NO2.index.weekday

```

```python
df_521_PM10['Weekday'] = df_521_PM10.index.weekday
df_521_PM25['Weekday'] = df_521_PM25.index.weekday
df_521_PM1['Weekday'] = df_521_PM1.index.weekday
```

```python
# df = df.groupby([df['date_UTC'].dt.hour, df['w_day']]).pm2_5_ugm3.mean().reset_index().dropna()
weekday_hourly_average_PM10_521 = df_521_PM10.groupby([df_521_PM10['Weekday'], df_521_PM10.index.hour]).Value.mean()
weekday_hourly_average_PM25_521 = df_521_PM25.groupby([df_521_PM25['Weekday'], df_521_PM25.index.hour]).Value.mean()
weekday_hourly_average_PM1_521 = df_521_PM1.groupby([df_521_PM1['Weekday'], df_521_PM1.index.hour]).Value.mean()
weekday_hourly_average_NO2_521 = df_521_NO2.groupby([df_521_NO2['Weekday'], df_521_NO2.index.hour]).Value.mean()
```

```python
weekday_hourly_75quantile_PM10_521 = df_521_PM10.groupby([df_521_PM10['Weekday'], df_521_PM10.index.hour]).Value.quantile(q=0.75)
weekday_hourly_25quantile_PM10_521 = df_521_PM10.groupby([df_521_PM10['Weekday'], df_521_PM10.index.hour]).Value.quantile(q=0.25)

weekday_hourly_75quantile_PM25_521 = df_521_PM25.groupby([df_521_PM10['Weekday'], df_521_PM25.index.hour]).Value.quantile(q=0.75)
weekday_hourly_25quantile_PM25_521 = df_521_PM25.groupby([df_521_PM10['Weekday'], df_521_PM25.index.hour]).Value.quantile(q=0.25)

weekday_hourly_75quantile_NO2_521 = df_521_NO2.groupby([df_521_NO2['Weekday'], df_521_NO2.index.hour]).Value.quantile(q=0.75)
weekday_hourly_25quantile_NO2_521 = df_521_NO2.groupby([df_521_NO2['Weekday'], df_521_NO2.index.hour]).Value.quantile(q=0.25)
```

```python
combined_dataframe = pd.DataFrame({'mean_PM10': weekday_hourly_average_PM10_521, 'mean_PM25': weekday_hourly_average_PM25_521, 'mean_NO2': weekday_hourly_average_NO2_521})
combined_dataframe_reset = combined_dataframe.reset_index(drop=True)
```

```python
fig, ax = plt.subplots(figsize=(20,5))
ax.plot(combined_dataframe_reset)
ax.fill_between(combined_dataframe_reset.index, weekday_hourly_25quantile_PM10_521, weekday_hourly_75quantile_PM10_521, facecolor='blue', alpha=0.2)
ax.fill_between(combined_dataframe_reset.index, weekday_hourly_25quantile_PM25_521, weekday_hourly_75quantile_PM25_521, facecolor='orange', alpha=0.2)
ax.fill_between(combined_dataframe_reset.index, weekday_hourly_25quantile_NO2_521, weekday_hourly_75quantile_NO2_521, facecolor='green', alpha=0.2)

ax.set_xticks(ax.get_xticks() + 10,["","Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "", ""])
fig.suptitle("Weekgang 521")
ax.legend(['PM10', 'PM2.5', 'NO2'])
ax.set_ylabel("concentraties [µg/m\xB3]")
# fig.show()
```




    Text(0, 0.5, 'concentraties [µg/m³]')



![png]({{ site.baseurl }}/Fine_dust_EDA_files/Fine_dust_EDA/Fine_dust_EDA_134_1.png){: .center-image }

```python
data = df_521_PM10.resample('60min').mean().ffill()
res = STL(data['Value'], period=24).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[1], line 1
    ----> 1 data = df_521_PM10.resample('60min').mean().ffill()
          2 res = STL(data['Value'], period=24).fit()
          3 res.plot()
    

    NameError: name 'df_521_PM10' is not defined


```python
df_2021['Datetime'] = pd.to_datetime(df_2021['Date'] + ' ' + df_2021['Time'])
df_2021 = df_2021.set_index('Datetime')

data = df_2021.resample('60min').mean().ffill()
```

```python
res = STL(data['PM10'].iloc[data.index > "2021-09-01"], period=24).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

```python
res = STL(data['PM10'].iloc[data.index > "2021-09-01"], period=168).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

# Weather Data
## Info
BRON: KONINKLIJK NEDERLANDS METEOROLOGISCH INSTITUUT (KNMI)
Opmerking: door stationsverplaatsingen en veranderingen in waarneemmethodieken zijn deze tijdreeksen van uurwaarden mogelijk inhomogeen! Dat betekent dat deze reeks van gemeten waarden niet geschikt is voor trendanalyse. Voor studies naar klimaatverandering verwijzen we naar de gehomogeniseerde dagreeksen <http://www.knmi.nl/nederland-nu/klimatologie/daggegevens> of de Centraal Nederland Temperatuur <http://www.knmi.nl/kennis-en-datacentrum/achtergrond/centraal-nederland-temperatuur-cnt>.

SOURCE: ROYAL NETHERLANDS METEOROLOGICAL INSTITUTE (KNMI)
Comment: These time series are inhomogeneous because of station relocations and changes in observation techniques. As a result, these series are not suitable for trend analysis. For climate change studies we refer to the homogenized series of daily data <http://www.knmi.nl/nederland-nu/klimatologie/daggegevens> or the Central Netherlands Temperature <http://www.knmi.nl/kennis-en-datacentrum/achtergrond/centraal-nederland-temperatuur-cnt>.

YYYYMMDD  = datum (YYYY=jaar,MM=maand,DD=dag) / date (YYYY=year,MM=month,DD=day)
HH        = tijd (HH=uur, UT.12 UT=13 MET, 14 MEZT. Uurvak 05 loopt van 04.00 UT tot 5.00 UT / time (HH uur/hour, UT. 12 UT=13 MET, 14 MEZT. Hourly division 05 runs from 04.00 UT to 5.00 UT
DD        = Windrichting (in graden) gemiddeld over de laatste 10 minuten van het afgelopen uur (360=noord, 90=oost, 180=zuid, 270=west, 0=windstil, 990=veranderlijk) / Mean wind direction (in degrees) for the 10-minute period preceding the observation time stamp (360=north, 90=east, 180=south, 270=west, 0=calm, 990=variable)
FH        = Windsnelheid (in 0.1 m/s) gemiddeld over het afgelopen uur / Mean wind speed (in 0.1 m/s) for the hour preceding the observation time stamp
FF        = Windsnelheid (in 0.1 m/s) gemiddeld over de laatste 10 minuten van het afgelopen uur / Mean wind speed (in 0.1 m/s) for the 10-minute period preceding the observation time stamp
"FX        = Hoogste windstoot (3 seconde gemiddelde windsnelheid; in 0.1 m/s) gemeten in het afgelopen uur / Maximum wind gust (3 second mean wind speed; in 0.1 m/s) in the preceding hour"
T         = Temperatuur (in 0.1 graden Celsius) op 1.50 m hoogte tijdens de waarneming / Temperature (in 0.1 degrees Celsius) at 1.50 m at the time of observation
T10N      = Minimumtemperatuur (in 0.1 graden Celsius) op 10 cm hoogte in de afgelopen 6 uur / Minimum temperature (in 0.1 degrees Celsius) at 0.1 m inthe preceding 6-hour period
TD        = Dauwpuntstemperatuur (in 0.1 graden Celsius) op 1.50 m hoogte tijdens de waarneming / Dew point temperature (in 0.1 degrees Celsius) at 1.50 m at the time of observation
SQ        = Duur van de zonneschijn (in 0.1 uren) per uurvak, berekend uit globale straling  (-1 for <0.05 uur) / Sunshine duration (in 0.1 hour) during the hourly division, calculated from global radiation (-1 for <0.05 hour)
Q         = Globale straling (in J/cm2) per uurvak / Global radiation (in J/cm2) during the hourly division
DR        = Duur van de neerslag (in 0.1 uur) per uurvak / Precipitation duration (in 0.1 hour) during the hourly division
RH        = Uursom van de neerslag (in 0.1 mm) (-1 voor <0.05 mm) / Hourly precipitation amount (in 0.1 mm) (-1 for <0.05 mm)
P         = Luchtdruk (in 0.1 hPa) herleid naar zeeniveau, tijdens de waarneming / Air pressure (in 0.1 hPa) reduced to mean sea level, at the time of observation
VV        = Horizontaal zicht tijdens de waarneming (0=minder dan 100m, 1=100-200m, 2=200-300m,..., 49=4900-5000m, 50=5-6km, 56=6-7km, 57=7-8km, ..., 79=29-30km, 80=30-35km, 81=35-40km,..., 89=meer dan 70km) / Horizontal visibility at the time of observation (0=less than 100m, 1=100-200m, 2=200-300m,..., 49=4900-5000m, 50=5-6km, 56=6-7km, 57=7-8km, ..., 79=29-30km, 80=30-35km, 81=35-40km,..., 89=more than 70km)
N         = Bewolking (bedekkingsgraad van de bovenlucht in achtsten), tijdens de waarneming (9=bovenlucht onzichtbaar) / Cloud cover (in octants), at the time of observation (9=sky invisible)
U         = Relatieve vochtigheid (in procenten) op 1.50 m hoogte tijdens de waarneming / Relative atmospheric humidity (in percents) at 1.50 m at the time of observation
WW        = Weercode (00-99), visueel(WW) of automatisch(WaWa) waargenomen, voor het actuele weer of het weer in het afgelopen uur. Zie https://cdn.knmi.nl/knmi/pdf/bibliotheek/scholierenpdf/weercodes_Nederland.pdf / Present weather code (00-99), description for the hourly division.
IX        = Weercode indicator voor de wijze van waarnemen op een bemand of automatisch station (1=bemand gebruikmakend van code uit visuele waarnemingen, 2,3=bemand en weggelaten (geen belangrijk weersverschijnsel, geen gegevens), 4=automatisch en opgenomen (gebruikmakend van code uit visuele waarnemingen), 5,6=automatisch en weggelaten (geen belangrijk weersverschijnsel, geen gegevens), 7=automatisch gebruikmakend van code uit automatische waarnemingen) / Indicator present weather code (1=manned and recorded (using code from visual observations), 2,3=manned and omitted (no significant weather phenomenon to report, not available), 4=automatically recorded (using code from visual observations), 5,6=automatically omitted (no significant weather phenomenon to report, not available), 7=automatically set (using code from automated observations)
M         = Mist 0=niet voorgekomen, 1=wel voorgekomen in het voorgaande uur en/of tijdens de waarneming / Fog 0=no occurrence, 1=occurred during the preceding hour and/or at the time of observation
R         = Regen 0=niet voorgekomen, 1=wel voorgekomen in het voorgaande uur en/of tijdens de waarneming / Rainfall 0=no occurrence, 1=occurred duringthe preceding hour and/or at the time of observation
S         = Sneeuw 0=niet voorgekomen, 1=wel voorgekomen in het voorgaande uur en/of tijdens de waarneming / Snow 0=no occurrence, 1=occurred during the preceding hour and/or at the time of observation
O         = Onweer 0=niet voorgekomen, 1=wel voorgekomen in het voorgaande uur en/of tijdens de waarneming / Thunder  0=no occurrence, 1=occurred during the preceding hour and/or at the time of observation
Y         = IJsvorming 0=niet voorgekomen, 1=wel voorgekomen in het voorgaande uur en/of tijdens de waarneming / Ice formation 0=no occurrence, 1=occurred during the preceding hour and/or at the time of observation

```python
eindhoven_weather_hourly = pd.read_csv('csv_files/uurgeg_370_2021-2030.csv', skiprows=31, sep='\,', engine='python')
for item in eindhoven_weather_hourly['"# STN'].index:
    eindhoven_weather_hourly['"# STN'][item] = int(eindhoven_weather_hourly['"# STN'][item][1:])
    eindhoven_weather_hourly['    Y"'][item] = int(eindhoven_weather_hourly['    Y"'][item][:-1])

eindhoven_weather_hourly = eindhoven_weather_hourly.rename(columns={'"# STN': '# STN', '    Y"': 'Y'})
columns = []
for col in eindhoven_weather_hourly.columns:
    columns.append(col.strip())
eindhoven_weather_hourly.columns = columns
eindhoven_weather_hourly_relevant = eindhoven_weather_hourly[['YYYYMMDD','HH', 'DD', 'FH', 'RH', 'T']]
eindhoven_weather_hourly_relevant.columns = ['Date', 'Hour', 'WindDir', 'WindSpeed_avg', 'PrecipationHour', 'Temp']
# eindhoven_weather_hourly_relevant.to_csv(df_file_path + 'hourdata_eindhoven_2021_16may2023.csv', index=False)
```

```python
eindhoven_weather_hourly_relevant.tail(1)
```

```python
eindhoven_weather_hourly = pd.read_csv('csv_files/uurgeg_370_2021-2030.csv', skiprows=31, skipinitialspace=True)
eindhoven_weather_hourly.tail()
```

```python
eindhoven_weather_hourly_full = pd.read_csv('csv_files/hourdata_eindhoven_2021_16may2023.csv')
eindhoven_weather_hourly_full
```

```python
weather_measurements_by_datetime = eindhoven_weather_hourly_relevant.copy()
datetimes = []
for index in weather_measurements_by_datetime.index:
    string_to_convert = str(weather_measurements_by_datetime['Date'][index]) + " " + str(weather_measurements_by_datetime['Hour'][index] - 1)
    datetimes.append(datetime.strptime(string_to_convert, "%Y%m%d %H"))

weather_measurements_by_datetime['DateTime'] = datetimes
print(weather_measurements_by_datetime.tail())
```

```python
measurement_df = pd.read_csv(df_file_path + 'full_measurement.csv')
measurement_df.tail()
```

```python
measurement_df['DateTime'] = pd.to_datetime(measurement_df['Date'] + ' ' + measurement_df['Time'])
measurement_df = measurement_df.set_index('DateTime', drop=True)
grouper = measurement_df.groupby([pd.Grouper(freq='1H'), 'Location'])\
    # .mean().ffill().asfreq('1H')
# hourly_measurement_full.sample(20)
result = grouper.mean().ffill()
```

```python
hourly_measurement_full = result.reset_index()
```

```python
weather_measurements_by_datetime
```

```python
combine_weather_measurements = hourly_measurement_full.copy()
combine_weather_measurements['WindDir'] = np.nan
combine_weather_measurements['WindSpeed_avg'] = np.nan
combine_weather_measurements['PrecipationHour'] = np.nan
combine_weather_measurements['Temp'] = np.nan

print(len(weather_measurements_by_datetime.index))
combine_weather_measurements_full = pd.DataFrame()
for index in weather_measurements_by_datetime.index:
    relevant_data = combine_weather_measurements[combine_weather_measurements['DateTime'] == weather_measurements_by_datetime['DateTime'][index]].copy()
    relevant_data['WindDir'] = weather_measurements_by_datetime['WindDir'][index]
    relevant_data['WindSpeed_avg'] = weather_measurements_by_datetime['WindSpeed_avg'][index]
    relevant_data['PrecipationHour'] = weather_measurements_by_datetime['PrecipationHour'][index]
    relevant_data['Temp'] = weather_measurements_by_datetime['Temp'][index]
    combine_weather_measurements_full = pd.concat([combine_weather_measurements_full, relevant_data])
    if index % 100 == 0:
        print(index)
```

```python
remaining = combine_weather_measurements[combine_weather_measurements['DateTime'] >= '2023-05-17 00:00:00']
```

```python
import requests as request
for date in remaining['DateTime'].map(lambda t: t.date()).unique():
    relevant_data = combine_weather_measurements[combine_weather_measurements['DateTime'].dt.date == date].copy()
    relevant_data['WindDir'] = weather_measurements_by_datetime['WindDir'][index]
    url = 'http://api.weatherapi.com/v1/history.json?key=2bd0e4d77ef74cf5bec123551231805&q=Eindhoven&dt=' + str(date)
    res = request.get(url)
    data = res.json()
    for hour in range(len(data['forecast']['forecastday'][0]['hour'])):
        relevant_hour = relevant_data[relevant_data['DateTime'].dt.hour == hour].copy()
        relevant_hour['WindDir'] = data['forecast']['forecastday'][0]['hour'][hour]['wind_degree']
        relevant_hour['WindSpeed_avg'] = data['forecast']['forecastday'][0]['hour'][hour]['wind_kph'] / 3.6
        relevant_hour['PrecipationHour'] = data['forecast']['forecastday'][0]['hour'][hour]['precip_mm']
        relevant_hour['Temp'] = data['forecast']['forecastday'][0]['hour'][hour]['temp_c']
        combine_weather_measurements_full = pd.concat([combine_weather_measurements_full, relevant_hour])

# combine_weather_measurements_full.to_csv(df_file_path + 'hour_combined_data_eindhoven_2021_19may2023.csv')
```

```python
current_data = combine_weather_measurements_full[combine_weather_measurements_full['Location'] == 521]
if pd.to_datetime(current_data.tail(1)['DateTime'].values[0]).date() < datetime.now().date():
    print(pd.to_datetime(current_data.tail(1)['DateTime'].values[0]).date())
    print(datetime.now().date())
elif pd.Timestamp(current_data.tail(1)['DateTime'].values[0]).hour < datetime.now().hour:
    print(pd.Timestamp(current_data.tail(1)['DateTime'].values[0]).hour)
    print(datetime.now().hour)

```

```python
last_datetime = current_data.tail(1).DateTime.values[0]
pd.to_datetime(last_datetime).strftime("%Y-%m-%dT%H:%M:%SZ")
```

```python
fig, ax = plt.subplots(figsize=(20,5))
ax.plot(combined_dataframe_reset)
ax.fill_between(combined_dataframe_reset.index, weekday_hourly_25quantile_PM10_521, weekday_hourly_75quantile_PM10_521, facecolor='blue', alpha=0.2)
ax.fill_between(combined_dataframe_reset.index, weekday_hourly_25quantile_PM25_521, weekday_hourly_75quantile_PM25_521, facecolor='orange', alpha=0.2)
ax.fill_between(combined_dataframe_reset.index, weekday_hourly_25quantile_NO2_521, weekday_hourly_75quantile_NO2_521, facecolor='green', alpha=0.2)

ax.set_xticks(ax.get_xticks() + 10,["","Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "", ""])
fig.suptitle("Weekgang 521")
ax.legend(['PM10', 'PM2.5', 'NO2'])
ax.set_ylabel("concentraties [µg/m\xB3]")
# fig.show()
```

```python
data = df_521_PM10.resample('60min').mean().ffill()
res = STL(data['Value'], period=24).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

```python
df_2021['Datetime'] = pd.to_datetime(df_2021['Date'] + ' ' + df_2021['Time'])
df_2021 = df_2021.set_index('Datetime')

data = df_2021.resample('60min').mean().ffill()
```

```python
res = STL(data['PM10'].iloc[data.index > "2021-09-01"], period=24).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```

```python
res = STL(data['PM10'].iloc[data.index > "2021-09-01"], period=168).fit()
res.plot()
plt.gcf().set_size_inches(20,10)
plt.show()
```
