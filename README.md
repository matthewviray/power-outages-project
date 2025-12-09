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

This is the head of my cleaned outage dataset:

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   DEMAND.LOSS.MW |   TOTAL.SALES |   TOTAL.CUSTOMERS |
|-------:|--------:|:-------------|:--------------|:-------------------|------------------:|---------------------:|-----------------:|--------------:|------------------:|
|   2011 |       7 | Minnesota    | MRO           | severe weather     |              3060 |                70000 |              nan |       6562520 |       2.5957e+06  |
|   2014 |       5 | Minnesota    | MRO           | intentional attack |                 1 |                  nan |              nan |       5284231 |       2.64074e+06 |
|   2010 |      10 | Minnesota    | MRO           | severe weather     |              3000 |                70000 |              nan |       5222116 |       2.5869e+06  |
|   2012 |       6 | Minnesota    | MRO           | severe weather     |              2550 |                68200 |              nan |       5787064 |       2.60681e+06 |
|   2015 |       7 | Minnesota    | MRO           | severe weather     |              1740 |               250000 |              250 |       5970339 |       2.67353e+06 |
# Univariate Plot 

<iframe
  src="assets/cause_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
