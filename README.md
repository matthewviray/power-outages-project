

# Power Outages: Analysis on Causes and Severity

# Introduction
In this project I explore a dataset of major power outages in the U.S ranging from January 200 all the way to July 2016 from many different states. The Departmenet of Energy categorized major outages as outages that either impacted at least 50,000 customers or had a unplanned firm load loss of at least 300MW. Containg a variety of features detailing the outage such as geographical, economic characteristics, electricy consumption, land-use, regional climatic info. 

This dataset is downloadable on: https://engineering.purdue.edu/LASCI/research-data/outages

We will explore this dataset to answer the question: **what drives the causes of our major power outages and its severity in the U.S.** We do this through data analysis techniques to see what drives these causes and the severity of these outages. Possibly seeing drivers from geographical, economic, land-use, electricity consumption, and regional climatic features. After our analysis and the understanding we gained from these dataset we will make a prediction model to predict if a outage is causes by weather or not.

This is crucial information because we are able to understand what drives the cause and severity of these outages, we are able to prepare and have a better chance to prevent these disasters. Also we are able to allocate resources more efficiently and effectively to tailor for the cause and needs of specific location in the U.S. 

## Introduction of our dataset
Our original dataset had 1534 rows of outages and their features and 57 columns that are prominent to my analysis and prediction model including features such as:

| **Feature** | **Description** |
|-------------|----------------|
| `YEAR` | Year of the outage event. |
| `MONTH` | Month of the outage event. |
| `U.S_STATE` | The U.S. state where the outage occurred. |
| `NERC.REGION` | North American Electric Reliability Corporation region of the outage. |
| `ANOMALY.LEVEL` | Numeric value representing climate anomaly level for the period/location. |
| `CLIMATE.CATEGORY` | Categorical climate classification of the region. |
| `CLIMATE.REGION` | Geographic climate region (e.g., Midwest, Northeast). |
| `OUTAGE.START.DATE` | Date when the outage began. |
| `OUTAGE.START.TIME` | Time when the outage began. |
| `OUTAGE.RESTORATION.DATE` | Date when power was restored. |
| `OUTAGE.RESTORATION.TIME` | Time when power was restored. |
| `CAUSE.CATEGORY` | Categorical cause of the outage (e.g., severe weather, equipment failure). |
| `OUTAGE.DURATION` | Duration of the outage (typically in hours). |
| `CUSTOMERS.AFFECTED` | Number of customers impacted by the outage. |
| `DEMAND.LOSS.MW` | Electrical demand lost during the outage (in megawatts). |
| `TOTAL.SALES` | Total electricity sales for the utility/region. |
| `TOTAL.CUSTOMERS` | Total number of customers served by the utility/region. |
| `UTIL.REALGSP` | Real gross state product — economic output associated with the utility/region. |
| `POPPCT_URBAN` | Percentage of population living in urban areas. |
| `POPDEN_URBAN` | Urban population density. |
| `AREAPCT_URBAN` | Percentage of geographic area classified as urban. |

# Data Cleaning and Exploratory Data Analysis 

## Data Cleaning
### Data Quality
I looked over random samples of our dataset to get a sense of our data and get an understanding of any values or specific columns. I also made sure the values are in agreement such as `OUTAGE.DURATION` and `OUTAGE.START.TIME`/`OUTAGE.RESTORATION.TIME`. Figurered which featured would be useful. For example `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED` as our measure for severity and `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`, `ANOMALY.LEVEL` , `CLIMATE.REGION`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`,`UTIL.REALGSP`, `POPPCT_URBAN`, `POPDEN_URBAN`, `AREAPCT_URBAN` as possible features that drive cause of outages and severity.

### Identifying and handling missing values
I looked through all the unique values and found rows that should be removed and also values that needed to be replace as they should be missing instead of having a value. 

- `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED` had values of 0 which doesen't make sense because if there was outage we would have a value greater than 0 for both of these features. So I replaced these values that had 0's with `np.nan`

- 9 rows of our data set had 9 or greater columns filled with np.nan. Most key features were missing that described or either would be useful for analysis so I decided to remove them. Here is what the rows looked like:

- There was one row where in the column `NERC.REGION` it had 2 regions instead of 1 so we got rid of that singular row so we only had one singular region for each row in that category. 

### Transformations

- I wanted to preform analyis/prediction modeling on weather outages compared to non weather outages to hopefully find insight and pattern between the two. I made a column `is_weather` which checked if `CAUSE.CATEGORY` is 'severe weather' or something else. If it is 'severe weather' the value of `is_weather` is 1 else 0.

- I also combined the data and time of both the start and restoration of outages and so I combined
`OUTAGE.START.DATE` and `OUTAGE.START.TIME` to make a new column `OUTAGE.START` that contains both the date and time. I did the same with `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` to make `OUTAGE.RESTORATION` We also dropped the old columns to make the new ones after. 

This is the head of my cleaned outage dataset and 8 of its columns:



| YEAR | MONTH | U.S._STATE | NERC.REGION | CAUSE.CATEGORY     | OUTAGE.DURATION | CUSTOMERS.AFFECTED | TOTAL.SALES |
|------|-------|------------|-------------|------------------|----------------|------------------|-------------|
| 2011 | 7     | Minnesota  | MRO         | severe weather    | 3060           | 70000            | 6562520     |
| 2014 | 5     | Minnesota  | MRO         | intentional attack| 1              | nan              | 5284231     |
| 2010 | 10    | Minnesota  | MRO         | severe weather    | 3000           | 70000            | 5222116     |
| 2012 | 6     | Minnesota  | MRO         | severe weather    | 2550           | 68200            | 5787064     |
| 2015 | 7     | Minnesota  | MRO         | severe weather    | 1740           | 250000           | 5970339     |



# Univariate Analysis 
In this univariate analysis we are seeing a histogram of the distribution of the causes of major outages.
This suggest we have very common causes of outages and also really rare causes of outages. Such as we see about half of our major outages is caused by severe weathered followed by intentional attacks. Where islanding and fuel supply emergency is the least likely cause of an outage. 

<iframe
  src="assets/cause_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Bivariate Analysis
In this bivariate analysis we the median outage duration of different causes of outages. We use median in this cause as the distribution of outage duration is skewed and there are many outliets in each category that makes the mean unrepresentative of the typical duration. We see the top causes of outage that leads to the most duration are severe weather and fuel supply emergency. 
<iframe
  src="assets/outage_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Interesting Aggregates

In this interesting aggregation we see that the different `NERC.REGIONS` could play a role on what outages are caused. THe distribution of the type of outages vary differently from North American Electric Reliability Corporation (NERC) regions meaning they all have their own characteristic that makes them either stronger or weaker for certain causes. 

| CAUSE.CATEGORY                |   ECAR |   FRCC |   HECO |   HI |   MRO |   NPCC |   PR |   RFC |   SERC |   SPP |   TRE |   WECC |
|:------------------------------|-------:|-------:|-------:|-----:|------:|-------:|-----:|------:|-------:|------:|------:|-------:|
| equipment failure             |      2 |      4 |      0 |    0 |     0 |      2 |    0 |     8 |      7 |     1 |     2 |     31 |
| fuel supply emergency         |      0 |      0 |      0 |    0 |     4 |     13 |    0 |     5 |      2 |     1 |     5 |     20 |
| intentional attack            |      1 |      2 |      0 |    0 |    16 |     57 |    0 |   106 |     30 |     8 |     9 |    189 |
| islanding                     |      0 |      0 |      0 |    0 |     2 |      1 |    0 |     5 |      1 |     2 |     0 |     35 |
| public appeal                 |      0 |      3 |      0 |    0 |     2 |      4 |    0 |     3 |     13 |    16 |    16 |     12 |
| severe weather                |     28 |     26 |      2 |    1 |    21 |     64 |    1 |   279 |    130 |    36 |    62 |    109 |
| system operability disruption |      3 |      8 |      1 |    0 |     0 |      9 |    0 |    12 |     18 |     2 |    17 |     55 |

# Assessment of missingness

## NMAR analysis

Customers Affected is NMAR data because the missingess is because of the value itself. One reason could be because different policies for different utilities. Such as companies might have either threshold and have specific polcies to report based on customers affected. For example, very high customers affected or really low customers affected may not be reported for some utilities as they could either delay or not report it at all. 
What it's reported on can fail if its an outlier of extreme values for customers affected or if the company think its too low they might choose not to report it at all. Due to the data collection process the data is NMAR. 


## Missing dependency

<iframe
  src="assets/climate_duration_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/climate_duration_missing_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


# Hypothesis testing 

<iframe
  src="assets/cause_by_month.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/cause_by_month_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


