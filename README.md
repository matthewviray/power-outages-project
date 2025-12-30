

# Power Outages: Analysis on Causes and Severity
Author: Matthew Viray

# Introduction
In this project I explore a dataset of major power outages in the U.S ranging from January 2000 all the way to July 2016 from many different states. The Departmenet of Energy categorized major outages as outages that either impacted at least 50,000 customers or had a unplanned firm load loss of at least 300MW. Containg a variety of features detailing the outage such as geographical, economic characteristics, electricy consumption, land-use, regional climatic info. 

This dataset is downloadable on: https://engineering.purdue.edu/LASCI/research-data/outages

We will explore this dataset to answer the question: **what drives the causes of our major power outages and its severity in the U.S.?** We do this through data analysis techniques to see what drives these causes and the severity of these outages. Possibly seeing drivers from geographical, economic, land-use, electricity consumption, and regional climatic features. After our analysis and the understanding we gained from these dataset we will make a prediction model to predict if a outage is caused by weather or not.

This is crucial information because we are able to understand what drives the cause and severity of these outages, we are able to prepare and have a better chance to prevent the causes and severity of these outages. Understanding the features of our dataset and how it connects to outages causes and severity, we are able to allocate resources more efficiently and effectively to tailor for the cause and needs of specific location in the U.S. In addition, focus on the weak points and find ways to improve those weak points to prevent these outages. 

## Introduction of our dataset
Our original dataset had 1534 rows of outages and their features and 20 columns that are prominent to my analysis and prediction model including features such as:

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
| `TOTAL.SALES` | Total electricity sales for the utility/region. |
| `TOTAL.CUSTOMERS` | Total number of customers served by the utility/region. |
| `UTIL.REALGSP` | Real gross state product/economic output associated with the utility/region. |
| `POPPCT_URBAN` | Percentage of population living in urban areas. |
| `POPDEN_URBAN` | Urban population density. |
| `AREAPCT_URBAN` | Percentage of land area classified as urban. |

# Data Cleaning and Exploratory Data Analysis 

## Data Cleaning
### Data Quality
I looked over random samples of our dataset to get a sense of our data and get an understanding of any values or specific columns. I also made sure the values are in agreement such as `OUTAGE.DURATION` and `OUTAGE.START.TIME`/`OUTAGE.RESTORATION.TIME`. Figurered which featured would be useful. For example `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED` as our measure for severity and `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`, `ANOMALY.LEVEL` , `CLIMATE.REGION`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`,`UTIL.REALGSP`, `POPPCT_URBAN`, `POPDEN_URBAN`, `AREAPCT_URBAN` as possible features that drive cause of outages and severity.

### Identifying and handling missing values
I looked through all the unique values and found rows that should be removed and also values that needed to be replace as they should be missing instead of having a value. 

- `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED` had values of 0 which doesen't make sense because if there was outage we would have a value greater than 0 for both of these features. So I replaced these values that had 0's with `np.nan`

- 9 rows of our data set had many columns filled with np.nan. Most key features were missing that described or either would be useful for analysis so I decided to remove them as they contain barley any info.

- There was one row where in the column `NERC.REGION`  had 2 regions instead of 1 so we got rid of that singular row so we only had one singular region for each row in that category. 

### Transformations

- I wanted to preform analyis/prediction modeling on weather outages compared to non weather outages to hopefully find insight and pattern between the two. I made a column `is_weather` which checks if `CAUSE.CATEGORY` is 'severe weather' or something else. If the cause is 'severe weather' the value of `is_weather` is 1 else 0.

- I also combined the data and time of both the start and restoration of outages and so I combined
`OUTAGE.START.DATE` and `OUTAGE.START.TIME` to make a new column `OUTAGE.START` that contains both the date and time. I did the same with `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` to make `OUTAGE.RESTORATION`. We also dropped the old columns to make the new ones after. 

This is the head of my cleaned outage dataset and 8 of its columns:


| YEAR | MONTH | U.S._STATE | NERC.REGION | CAUSE.CATEGORY     | OUTAGE.DURATION | CUSTOMERS.AFFECTED | TOTAL.SALES |
|------|-------|------------|-------------|------------------|----------------|------------------|-------------|
| 2011 | 7     | Minnesota  | MRO         | severe weather    | 3060           | 70000            | 6562520     |
| 2014 | 5     | Minnesota  | MRO         | intentional attack| 1              | nan              | 5284231     |
| 2010 | 10    | Minnesota  | MRO         | severe weather    | 3000           | 70000            | 5222116     |
| 2012 | 6     | Minnesota  | MRO         | severe weather    | 2550           | 68200            | 5787064     |
| 2015 | 7     | Minnesota  | MRO         | severe weather    | 1740           | 250000           | 5970339     |



# Univariate Analysis 
In this univariate analysis we are analyzing a histogram of the distribution of the causes of major outages.
This suggest we have very common causes of outages and also really rare causes of outages. Such as we see about half of our major outages is caused by severe weather followed by intentional attacks. Where islanding and fuel supply emergency is the least likely cause of an outage. 

<iframe
  src="assets/cause_distribution.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

In this second univariate analysis we see a bar plot that explains the distribution of outages in which NERC Region they appear in. This showing WECC having the most outage, followed by RFC. In contrast PR with the least outages and second to that is HI. These regions have drastic different numbers of outages that can suggest different characteristics per region such as weather or infrastructure that impacts the frequency of outages. 

<iframe
  src="assets/nerc_region.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

This map reveals the distribution of outages by the climate regions in the U.S. We see climate regions such as Northeast and West having many outages and regions that are the opposite with only a few outages such as Southwest and West North Central. Suggesting that climate region has impact on outages happening due to their different features, such as geographical, weather, and etc. 

<iframe
  src="assets/climate_region_outage_count_red_map_with_AK_HI.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

# Bivariate Analysis
In this bivariate analysis, the box plots of median outage duration for different causes of outages are shown. We use median in this as the distribution of outage duration is skewed and there are many outliers in each category that makes the mean unrepresentative of the typical duration. We see the top causes of outage that leads to the most duration are severe weather and fuel supply emergency. 

<iframe
  src="assets/outage_cause.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

In this second bivariate analysis I use the median outage duration by month revealing how each month has their own characteristic, prominent ones such as weather due to also patterns seen through multiple months due to different seasons causing more or less outages. We can seen this as the months go by in the plots where there are increasing and decreasing patterns.
(1-January,2-Febuary,...,11-Novemnber,12-December)

<iframe
  src="assets/med_duration.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

# Interesting Aggregates

In this interesting aggregation we see that the different `NERC.REGIONS` could play a role on what outages are caused. The distribution of the type of outages vary differently from North American Electric Reliability Corporation (NERC) regions meaning they all have their own characteristic such as economic outpu towards utiity and infrastructure or geographical characteristic that makes them either stronger or weaker for certain causes. 

| CAUSE.CATEGORY                |   ECAR |   FRCC |   HECO |   HI |   MRO |   NPCC |   PR |   RFC |   SERC |   SPP |   TRE |   WECC |
|:------------------------------|-------:|-------:|-------:|-----:|------:|-------:|-----:|------:|-------:|------:|------:|-------:|
| equipment failure             |      2 |      4 |      0 |    0 |     0 |      2 |    0 |     8 |      7 |     1 |     2 |     31 |
| fuel supply emergency         |      0 |      0 |      0 |    0 |     4 |     13 |    0 |     5 |      2 |     1 |     5 |     20 |
| intentional attack            |      1 |      2 |      0 |    0 |    16 |     57 |    0 |   106 |     30 |     8 |     9 |    189 |
| islanding                     |      0 |      0 |      0 |    0 |     2 |      1 |    0 |     5 |      1 |     2 |     0 |     35 |
| public appeal                 |      0 |      3 |      0 |    0 |     2 |      4 |    0 |     3 |     13 |    16 |    16 |     12 |
| severe weather                |     28 |     26 |      2 |    1 |    21 |     64 |    1 |   279 |    130 |    36 |    62 |    109 |
| system operability disruption |      3 |      8 |      1 |    0 |     0 |      9 |    0 |    12 |     18 |     2 |    17 |     55 |

In this second aggregation, we get to see significant statistic of each cause category. For example `CUSTOMERS.AFFECTED`, `OUTAGE.DURATION`, `OUTAGE_COUNT`, `TOTAL.CUSTOMERS`, `TOTAL.SALES`, `UTIL.REALGSP`. We use the median to find the typical value of each of these features as many are non normally distributed. From this aggregation we get to see how these features correlate especially finding connections between severity, economic characteristics, and the cause categories. For example, severe weather has a low `UTIL.REALGSP`(Utility industry economic output) compared to other causes but has the highest median amount of customers affected. 

| CAUSE.CATEGORY                |   CUSTOMERS.AFFECTED |   OUTAGE.DURATION |   OUTAGE_COUNT |   TOTAL.CUSTOMERS |   TOTAL.SALES |   UTIL.REALGSP |
|:------------------------------|---------------------:|------------------:|---------------:|------------------:|--------------:|---------------:|
| equipment failure             |                51750 |             224   |             57 |       9.62991e+06 |   1.76482e+07 |          16403 |
| fuel supply emergency         |                    1 |            3960   |             50 |       8.12107e+06 |   1.44e+07    |          19828 |
| intentional attack            |                 2500 |              92.5 |            418 |       2.71339e+06 |   5.17755e+06 |           3832 |
| islanding                     |                 4300 |              77.5 |             46 |       1.48323e+07 |   1.95601e+07 |          26426 |
| public appeal                 |                15800 |             455   |             69 |       4.78342e+06 |   1.08462e+07 |           8716 |
| severe weather                |               111600 |            2464   |            759 |       4.7874e+06  |   1.00348e+07 |           8332 |
| system operability disruption |                69000 |             220   |            125 |       9.62991e+06 |   1.93772e+07 |          18761 |


# Assessment of missingness

In our cleaned dataset we have a few features with significant missing values such as `CUSTOMERS.AFFECTED` and `OUTAGE.DURATION`

## NMAR analysis

Customers Affected is NMAR data because the missingess is because of the value itself. One reason could be because different policies for different utility companies. Such as companies might have either threshold and have specific polcies to report based on customers affected. For example, very large customers affected or really low customers affected may not be reported for some utilities as they could either delay or not report it at all. This is possible because major outages are reported either due to high customers affected or a high firm load loss. Another reason could be it's reported on can fail if its an outlier of extreme values for customers affected or if the company think its too low they might choose not to report it at all.


## Missing dependency

### Climate Region
We look at the `CLIMATE.REGION` column to analyze the missingness of `OUTAGE.DURATION` as it contains missing values
which we'll preform a permutation test to see if its missigness depends on `CLIMATE.REGION`. First I plotted the 
distribution of `CLIMATE.REGION` that are missing and not missing. Then we will preformm a permutation test to test our hypotheses of:

### Null Hypothesis: 
The distribution of `CLIMATE.REGION` is the same between the two groups, missing and non missing `OUTAGE.DURATION` values.

### Alternate Hypothesis: 
The distribution of `CLIMATE.REGION` is different between the two groups, missing and non missing `OUTAGE.DURATION` values.

### Test statistic: 
The Total Variation Distance between the distribution of `CLIMATE.REGION` of the two groups, missing and non missing `OUTAGE.DURATION` values.

<iframe
  src="assets/climate_duration_missing.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Results from our permutation test:
From our permutation test that I preformed with 1000 shuffles in the `OUTAGE.DURATION` values. We reject the null hypothesis of `CLIMATE.REGION` distribution being the same between missing an non missing `OUTAGE.DURATION` values. We reject the null because we got a p-value of 0.00. So, the missigness of `OUTAGE.DURATION` depends on `CLIMATE.REGION`. Seen from the observed statistic in the distribution of tvd it's less than the significance level of 0.05. 

<iframe
  src="assets/climate_duration_missing_distribution.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Percentage of Total Population in the State that's in a urban area
We look at the `POPPCT_URBAN` column to analyze the missingness of `OUTAGE.DURATION` as it contains missing values
which we'll preform a permutation test to see if its missigness depends on `POPPCT_URBAN`. First I plotted the 
distribution of `AREAPCT_URBAN` that are missing and not missing. Then we will preformm a permutation test to test our hypotheses of:

### Null Hypothesis: 
The distribution of `POPPCT_URBAN` is the same between missing and non missing `OUTAGE.DURATION` values.

### Alternate Hypothesis: 
The distribution of `POPPCT_URBAN` is different between missing and non missing `OUTAGE.DURATION` values.

### Test statistic: 
The K-S stat(Maximum difference between two numerical cumulative distribution) between the distribution of `POPPCT_URBAN` of missing and non missing `OUTAGE.DURATION` values.

<iframe
  src="assets/k2samp_urban_missingness.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Results from our K-S test:
From our test that I preformed using `ks_2samp`. With `ks_2samp` I use the Kolmogorov–Smirnov (KS) statistic for my permutation test in order to get a p-value to test my hypothesis with a 0.05 significance level. The p-value from my permutation test is 0.084 with 1000 repetitions. So, I fail to reject the null suggesting the distribution of `POPPCT_URBAN` is the same between missing and non missing `OUTAGE.DURATION` values. Meaning the missingness of `OUTAGE.DURATION` is not dependent on `POPPCT_URBAN`.

<iframe
  src="assets/k2samp_urban_missingness_distribution.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

# Hypothesis testing 

A hypothesis test that will have insight on the impact specific months have on causes of outages is testing the distribution of 2 months. I decided to choose January and July because both reflect different weather, seasons, and etc. January representing Winter and July representing Summer. Choosing 2 completely opposite Months due to their complete opposite characteristics due to season and the weather/temperature that comes with it. To investigate what drives the causes of outages we preform a permutation test to see if specific Months drives different causes of outages. A permutation test with a test statistic of TVD to see if the distribution of causes in these 2 are different. We need TVD to quantify the difference between distributions and compare it by preforming a permutation test with a TVD test statistic to see if the difference is due to chance or there is a difference as in a obsereved statistic below the p-value of 0.05.

### Null Hypothesis: 
The distribution of outages in the cause categories is the same in January and July and the difference is due to chance.

### Althernate Hypothesis: 
The distribution of outages in the cause categories is different in January and July.

### Test Statistic: 
The Total Variation Distance between the distribution of `CLIMATE.REGION` between January and July.

<iframe
  src="assets/cause_by_month.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Results: 
With a p-value of 0.005 and a significance value of 0.05 for our test. We reject the null hypothesis and it suggest that the distribution of causes of our outages is different between January and June. This test and rejecting the null suggest that month may have a impact on what drives the cause of an outage. Where its possible for different months changes the frequency of the causes of outages to be more common or rare depending on what month it is. 
<iframe
  src="assets/cause_by_month_distribution.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

# Framing a prediction problem 

I plan to predict if the cause of a outage is due to weather or non-weather causes. This prediction will be useful to better pepare ahead of time and allocate resourses to the right locations based on the knowledge if in the future the outages are going to be weather related or not, especially as about half of the outages in the data is due to severe weather. This prediction problem will be classification and the type of classification it'll be is binary classification.  

The variable we'll be predicting in our cleaned dataset is `is_weather` as the column has a 1 if the caused is weather realted else 0. We choose accuracy as our metric score because our dataset is balanced where accuracy wont be bias towards a major category. In our prediction model we can only use features that we have at the time of prediction so we'll try and use `MONTH`, `U.S._STATE`, `NERC.REGION`, `ANOMALY.LEVEL` , `CLIMATE.CATEGORY`, `CLIMATE.REGION`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`,`UTIL.REALGSP`, `POPPCT_URBAN`, `POPDEN_URBAN`, and`AREAPCT_URBAN` for our prediction model.

# Base Model

The base model I made is with a decision tree classifier, and it includes the features `U.S._STATE`(NOMINAL), `MONTH`(ORDINAL), `NERC.REGION`(NOMINAL), and `CLIMATE.REGION`(NOMINAL) in order to predict if an outage is weather related or not. With the 4 of these features, for me to use it in my decision tree classifier I had to encode it by doing one hot encoding, making a column for each unique value of each feature and filled with 1 if they have that unique value else 0. 

I choose these four features because: 

- `U.S._STATE`: From different states many consist of their own and unique features. Such as having different environments, intensity of weather, and  infrastructure. For example Texas and Florida with hurricans possible causing outages. New York and Minnesota having freeing weather and possible severe snow. 

- `MONTH`: There are many weather related impact based on each month. In different months many different weather(Heavy Rain, tornados, Storms) can occur for each month making each month contribute to the chance of an outage being weather related. So, many of these months have different weather patterns that is likely to increase or decrease the chances for weather related outages to happen. 

- `NERC.REGION`: Each region share electric grid characteristic and infrastructure design that manage and organize the power system. The weakness and strengths of each region is shared for these different regions which influence the cause of outage depending on what characteristic these regions contain and how weak they are compared to the different causes of outages.

- `CLIMATE.REGION`: Different climate regions in the U.S. that consist of their own weather, vegetation, and grid infrastructure that are more likely to be imapcted by severe weather depending on the region .

The preformance of my model was a accuracy score of 0.685 on the test set. Meaning about 68% of the time my prediction model was able to predict of the cause of an outage is either weather related or not. I feel that our prediction model did alright and we can improve it trying a different model, adding more features, and searching for the best hyperparemeters which we do in the next section 

# Final Model

The new features we added in our final model are:

- `YEAR`(Ordinal): Every year there are different patterns and causes that could increase or decrease the chance of specific causes of outages. These patterns and trends can be seen from multiple years possiblly predict cause of future outages from many reasons such as advancement in technology, weather/climate/enviroment changes, human factors, and etc.

- `ANOMALY.LEVEL`(Quantitative): Numerical feature that describes the weather and climate which could influence whethere an outage will be from severe weather as the `ANOMALY.LEVEL` increase or decrease. 

- `utility_score`(Quantitative): Our utility score is based on `UTIL.REALGSP` and `TOTAL.CUSTOMERS` where we divided `UTIL.REALGSP` with `TOTAL.CUSTOMERS`. We made this feature as we had 3 columns that are redundant and all describe the utility characteristic in our outages. So we combined two of them to make a utility score that takes account of the customers while also describing the economic output into utility of these places per customer. A high utility score reveals a stronger and better funded utility that is usually better at handling against weather compared to weaker utilities as they more vulnerable due and less money is spent on it.  

- `urban_score`(Quantitative): Same with our urban factors we had a lot of factors that were redundant that describes how urban the place is. So to describe how urban a place is with one feature we made a score using `POPDEN_URBAN` and `POPPCT_URBAN`. The higher the urban score is the more area is urban and the more dense those area are. How urban the area helps us understand how vulnerable an area is to weather outages due to many interconnected infrastructure covering more area or a better possibility of failure due to lots of infrastructure meaning more places for failure.

- `SEASON`(Nominal): We have our month column that finds insight on specific months mostly use for finding weather that influence from a single month. However, with seasons where we mapped each month to a specific season(Fall, Spring, Winter, Summer) we're able to see how each season due to their different weather influence weather outage. Seeing how specific group of months by season influece cause of outages due to the weather thats possible in those seasons. 

For our final model I decided to use a Random Forest Classifier as our first model was a Decision tree model. I used GridSearchCV to decide which hyperparameters to choose for my model. The hyperparameters we decided on were:

| Hyperparameter          | Description                                 | Values Tested      |
| ----------------------- | ------------------------------------------- | ------------------ |
| **`n_estimators`**      | Number of trees in the forest               | 100, 200, 350, 500 |
| **`max_depth`**         | Maximum depth of each tree                  | 5, 10, 15, 20      |
| **`min_samples_split`** | Minimum samples required to split a node    | 2, 4, 6, 8         |
| **`min_samples_leaf`**  | Minimum samples required at a leaf node     | 1, 2, 3, 4         |
| **`max_features`**      | Number of features considered at each split | "sqrt", 0.5        |

From our GridSearchCV results, we chose:

- `randomforestclassifier__max_depth`: 10  
- `randomforestclassifier__max_features`: 0.5  
- `randomforestclassifier__min_samples_leaf`: 1  
- `randomforestclassifier__min_samples_split`: 6  
- `randomforestclassifier__n_estimators`: 200  

In my final model the accuracy 0.821 is now on our test set, which increased by around 0.146. Meaning our final model was able to predict correctly 82.1% of the time and improved by 14.6% from our base model. This final model has improved a lot from our base model, and this is a result from our improvement in accuracy in our final model. 

# Fairness Analysis

For the fairness analysis to see if my prediction will preform differently between two groups I decided to do it with areas with High utility investment and economic output comapred to low utility investment and economic output. This is crucial as the higher the investment, the more strong the infrastructure and services around utility will be to withstand against weather related outages. Comparing these two will help understand if our model is able to fairly make prediction between high utility and low utility. 

We seperated our `UTIL.REALGSP` column into two groups by getting the median and seperating the two. Where the low utility group will be everything below the median and the high utility group is everything above the median. 

## Null Hypothesis: 
Model is fair in terms of recall between high utility economic output and low utility economic output.

## Alternative Hypothesis:
Model is not fair in terms of recall between high utility economic output and low utility economic outputs.

## Test Statistic: 
Our test statistic will be the absolute difference of recall between each group because we want to prioritize making sure we minimize false negative as we want to make sure that outages in the future that are due to severe weather are allocated and prepared for before any severe impact it has on the area.

## Results: 
I preformed a permutation test with 5000 repetitions and with a significance level of 0.05. We got a p-value of 0.978 revealing that we fail to reject that our model is fair in recall when comparing high utility groups versus low utility groups.

<iframe
  src="assets/permutation_test_fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

