

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

In our cleaned dataset we have a few features with significant missing values such as `CUSTOMERS.AFFECTED` and `OUTAGE.DURATION`

## NMAR analysis

Customers Affected is NMAR data because the missingess is because of the value itself. One reason could be because different policies for different utilities. Such as companies might have either threshold and have specific polcies to report based on customers affected. For example, very high customers affected or really low customers affected may not be reported for some utilities as they could either delay or not report it at all. 
What it's reported on can fail if its an outlier of extreme values for customers affected or if the company think its too low they might choose not to report it at all. Due to the data collection process the data is NMAR. 


## Missing dependency

### Climate Region
We look at the `CLIMATE.REGION` column to analyze the missingness of `OUTAGE.DURATION` as it contains missing values
which we'll preform a permutation test to see if its missigness depends on `CLIMATE.REGION`. First I plotted the 
distribution of `CLIMATE.REGION` that are missing and not missing. Then we will preformm a permutation test to test our hypotheses of:

### Null Hypothesis: The distribution of `CLIMATE.REGION` is the same between missing and non missing `OUTAGE.DURATION` values

### Alternate Hypothesis: The distribution of `CLIMATE.REGION` is different between missing and non missing `OUTAGE.DURATION` values

### Test statistic: The Total Variation Distance between the distribution of `CLIMATE.REGION` of missing and non missing `OUTAGE.DURATION` values
<iframe
  src="assets/climate_duration_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Results from our permutation test:
From our permutation test that I preformed with 1000 shuffles in the `OUTAGE.DURATION` values. We reject the null hypothesis of `CLIMATE.REGION` distribution being the same between missing an non missing `OUTAGE.DURATION` values. We reject it because we got a p-value of 0.00. So, the missigness of `OUTAGE.DURATION` depends on `CLIMATE.REGION`. Seen from the observed statistic in the distribution of tvd it's less than the significance level of 0.05. 
<iframe
  src="assets/climate_duration_missing_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


# Hypothesis testing 

A hypothesis test that will have insight on the impact specific months have on causes of outages is testing the distribution of 2 months. I decided to choose January and July because both reflect different weather, seasons, and etc. January representing winter and July representing Summer. Choosing 2 completely opposite Months due to their complete opposite characteristics due to season and the weather/temperature that comes with it. To investigate what drives the causes of outages we preform a permutation test to see if specific Months drives different causes of outages. A permutation test with a test statistic of TVD because to see if the distribution of causes in these 2 are different we need TVD to quantify the difference between distributions and compare it to a distribution of difference if they weren't different to see if the difference is due to chance or there is a difference as in a obsereved statistic below the p-value of 0.05.

## Null Hypothesis: The distribution of outages in the cause categories is the same in January and July and the difference is due to chance.

## Althernate Hypothesis: The distribution of outages in the cause categories is different in January and July.

## Test Statistic: The Total Variation Distance between the distribution of `CLIMATE.REGION` between January and July.

<iframe
  src="assets/cause_by_month.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Results: 
With a p-value of 0.005 and a significance value of 0.05 for our test. We reject the null hypothesis and it suggest that the distribution of causes of our outages is different between January and June. This test and rejecting the null suggest that month have a impact on what drives the cause of an outage. Where, its possible for different months the causes of outages are more common or rare depending on what month it is. 
<iframe
  src="assets/cause_by_month_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Framing a prediction problem 

I plan to predict if the cause of a outage is due to weather or non-weather causes. This prediction will be useful to better pepare ahead of time and allocate resourses to the right locations based on the knowledge if in the future the outages are going to be weather related or not, especially as about half of the outages in the data is due to severe weather. This prediction problem will be classification and the type of classification it'll be is binary classification.  

The variable we'll be predicting in our cleaned dataset is `is_weather` as the column has a 1 if the caused is weather realted else 0. We choose accuracy as our metric score because our dataset is balanced where accuracy wont be bias towards a major category. In our prediction model we can only use features that we have at the time of prediction so we'll try and use `MONTH`, `U.S._STATE`, `NERC.REGION`, `ANOMALY.LEVEL` , `CLIMATE.REGION`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`,`UTIL.REALGSP`, `POPPCT_URBAN`, `POPDEN_URBAN`, and`AREAPCT_URBAN` for our prediction model.

# Base Model

The base model I made is with a decision tree classifier, and it includes the features `U.S._STATE`(NOMINAL), `MONTH`(ORDINAL), `NERC.REGION`(NOMINAL), and `CLIMATE.REGION`(NOMINAL) in order to predict if an outage is weather related or not. With the 4 of these features, for me to use it in my decision tree classifier I had to encode it by doing one hot encoding, making a column for each unique value of each feature and filled with 1 if they have that unique value else 0. 

I choose these four features because: 

- `U.S._STATE`: From different states many consist of their own and unique features. Such as having different environments, intensity of weather, and  infrastructure. For example Texas and Florida with hurricans possible causing outages. New York and Minnesota having freeing weather and possible severe snow. 
- `MONTH`: There are many weather related impact based on each month. In different months many different weather(Heavy Rain, tornados, Storms) can occur for each month making each month contribute to the chance of an outage being weather related. So, many of these months have different weather patterns that is likely to increase or decrease the chances for weather related outages to happen. 

- `NERC.REGION`: Each region share electric grid characteristic and infrastructure design that manage and organize the power system. The weakness and strengths of each region is shared for these different regions which influence the cause of outage depending on what characteristic these regions contain and how weak they are compared to the different causes of outages

- `CLIMATE.REGION`: Different climate regions in the U.S. that consist of their own weather, vegetation, and grid infrastructure that are more likely to be imapcted by severe weather depending on the region 

The preformance of my model was a accuracy score of 0.685 on the test set. Meaning about 68% of the time my prediction model was able to predict of the cause of an outage is either weather related or not. I feel that our prediction model did alright and we can improve it trying a different model, adding more features and searching for the best hyperparemeters which we do in the next section 



# Final Model

- `YEAR`: Every year there are different patterns and causes that could increase or decrease the chance of specific causes of outages. Especially as technology and equpiment evolves and improves there is a decrease of certain outages causes which these trends and patterns can show in years.

- `ANOMALY.LEVEL`

- `URBAN SCORE`

- `UTILITY SCORE`

- 

# Fairness Analysis