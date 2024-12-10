# Analyzing Power Outages
Project for Dsc 80 at UCSD

By Kevin Zhang

# Introduction

This project analyzes a dataset of major power outages in the U.S. from January 2000 to July 2016, accessed from Purdue University's Laboratory for Advancing Sustainable Critical Infrastructure. Major outages, as defined by the Department of Energy, are those affecting at least 50,000 customers or resulting in an unplanned energy demand loss of at least 300 MW. The dataset includes details on outages, geographic and climatic factors, land-use characteristics, electricity consumption patterns, and the economic attributes of affected states.

The analysis begins with data cleaning and exploratory data analysis to understand the dataset, followed by an investigation of missing data patterns and dependencies. The research focuses on identifying key causes and characteristics of major outages and building a predictive model to determine outage causes based on time and location. Such insights are critical for enabling energy companies to take preventive measures, such as reinforcing infrastructure or improving security.

The original dataset contains 1534 rows (outages) and 57 columns, but this analysis will focus on selected columns relevant to the research objectives.

| Column                  | Description                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| `'YEAR'`               | Year when the outage occurred.                                              |
| `'MONTH'`              | Month when the outage occurred.                                             |
| `'U.S._STATE'`         | State where the outage occurred.                                            |
| `'NERC.REGION'`        | North American Electric Reliability Corporation (NERC) region involved in the outage. |
| `'CLIMATE.REGION'`     | U.S. Climate region as specified by the National Centers for Environmental Information (9 regions). |
| `'ANOMALY.LEVEL'`      | Oceanic El Niño/La Niña (ONI) index indicating cold and warm seasonal episodes. |
| `'OUTAGE.START.DATE'`  | Date when the outage event started.                                         |
| `'OUTAGE.START.TIME'`  | Time of day when the outage event started.                                  |
| `'OUTAGE.RESTORATION.DATE'` | Date when power was fully restored.                                     |
| `'OUTAGE.RESTORATION.TIME'` | Time of day when power was fully restored.                              |
| `'CAUSE.CATEGORY'`     | Category of events that caused the outage.                                  |
| `'OUTAGE.DURATION'`    | Duration of the outage (in minutes).                                        |
| `'DEMAND.LOSS.MW'`     | Peak demand lost during the outage (in Megawatts).                          |
| `'CUSTOMERS.AFFECTED'` | Number of customers affected by the outage.                                 |
| `'TOTAL.CUSTOMERS'`    | Annual number of total electricity customers in the state.                  |


# Data Cleaning and Exploratory Data Analysis

To clean my dataset, I import the raw data (excel file) and do several
pandas operations to prepare it for analysis. 

## Cleaning

First, I keep only the columns relevant for my analysis: YEAR, MONTH, U.S._STATE, NERC.REGION, CLIMATE.REGION, ANOMALY.LEVEL, OUTAGE.START.DATE, OUTAGE.START.TIME, OUTAGE.RESTORATION.DATE, OUTAGE.RESTORATION.TIME, CAUSE.CATEGORY, OUTAGE.DURATION, DEMAND.LOSS.MW, CUSTOMERS.AFFECTED, and TOTAL.CUSTOMERS. All other columns are dropped as they are not needed for this analysis.

OUTAGE.START.DATE and OUTAGE.START.TIME are combined into a single column called OUTAGE.START, and similarly, I combine OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME into OUTAGE.RESTORATION. These new columns are stored as timestamp objects, and the original date and time columns are dropped since their information is now consolidated.

Finally, I examine the key outcome variables of interest: OUTAGE.DURATION, CUSTOMERS.AFFECTED, and DEMAND.LOSS.MW. Values of 0 in these columns are likely placeholders for missing data, as major outages wouldn't have 0 duration, 0 customers affected, or 0 MW lost. I replace these 0 values with np.nan to mark them as missing data.

The head of the cleaned dataframe (with column subset) is as follows. 

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   OUTAGE.DURATION |
|-------:|--------:|:-------------|:--------------|:-------------------|------------------:|
|   2011 |       7 | Minnesota    | MRO           | East North Central |              3060 |
|   2014 |       5 | Minnesota    | MRO           | East North Central |                 1 |
|   2010 |      10 | Minnesota    | MRO           | East North Central |              3000 |
|   2012 |       6 | Minnesota    | MRO           | East North Central |              2550 |
|   2015 |       7 | Minnesota    | MRO           | East North Central |              1740 |

## Exploratory Data Analysis

### Univariate Analysis

To begin my EDA, I first perform univariate analysis to examine the distribution of single variables. 

First, I wanted to see the distribution of outage durations. 

<iframe
  src="assets/outage_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I also looked at the major causes of outages

<iframe
  src="assets/major_causes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then, I created a map in Folium to visualize the occurences of power outage by state
<iframe
  src="assets/map1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Bivariate Analysis

I also conducted a couple bivariate analyses.

I examined the relationship between Outage Duration and Climate Region. I expected regions prone to extreme weather, such as hurricanes or snowstorms, to have longer outages due to greater infrastructure damage. Conversely, regions with milder climates or higher urbanization might experience shorter outages due to fewer severe events and quicker restoration efforts.

<iframe
  src="assets/climte_region_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot below shows the relation between outage duration and customers affected. Most of the data 
is clusted in the bottom left corner, but there are a notable amount of outliers. 
<iframe
  src="assets/customers_affected_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Grouping and Aggregates

| CLIMATE.REGION       | OUTAGE.DURATION   | CUSTOMERS.AFFECTED   | DEMAND.LOSS.MW   |
|:---------------------|------------------:|---------------------:|-----------------:|
| Central              | 2882.21          | 144269               | 574.362          |
| East North Central   | 5391.4           | 149816               | 633.902          |
| Northeast            | 3330.52          | 175359               | 981.359          |
| Northwest            | 1536.36          | 148472               | 343.933          |
| South                | 2872.45          | 206106               | 471.648          |
| Southeast            | 2247.66          | 198593               | 852.358          |
| Southwest            | 1621.41          | 87815.1              | 909.762          |
| West                 | 1636.31          | 225303               | 717.932          |
| West North Central   | 796.071          | 66242.4              | 391.2            |



|    |   YEAR |   OUTAGE.DURATION (mean) |   OUTAGE.DURATION (count) |   DEMAND.LOSS.MW (mean) |   DEMAND.LOSS.MW (sum) |   CUSTOMERS.AFFECTED (mean) |   CUSTOMERS.AFFECTED (sum) |
|----|--------|---------------------------|----------------------------|-------------------------|------------------------|-----------------------------|----------------------------|
|  0 |   2000 |                   3080    |                         12 |                295.786  |                 4141   |                    203361   |               4.27058e+06 |
|  1 |   2001 |                   1272.07 |                         14 |                390.556  |                 3515   |                    178926   |               1.43141e+06 |
|  2 |   2002 |                   4751    |                         14 |                228.333  |                 2740   |                    425506   |               6.38259e+06 |
|  3 |   2003 |                   4652.43 |                         46 |               1987.95   |                79518   |                    303978   |               1.24631e+07 |
|  4 |   2004 |                   4368.79 |                         71 |                895.161  |                55500   |                    222829   |               1.35926e+07 |
|  5 |   2005 |                   5288.94 |                         54 |                739.324  |                27355   |                    301157   |               1.35521e+07 |
|  6 |   2006 |                   3329.53 |                         66 |                347.571  |                17031   |                    178107   |               1.01521e+07 |
|  7 |   2007 |                   2336.67 |                         54 |                321.176  |                10920   |                    149336   |               5.97343e+06 |
|  8 |   2008 |                   4184.02 |                        110 |                473.629  |                33154   |                    212393   |               1.99649e+07 |
|  9 |   2009 |                   3660.52 |                         77 |                277.5    |                12765   |                    133598   |               6.81348e+06 |
| 10 |   2010 |                   2965.5  |                        105 |                345.711  |                15557   |                    136596   |               1.01081e+07 |
| 11 |   2011 |                   2192.9  |                        221 |                594.268  |                42193   |                    134743   |               1.64387e+07 |
| 12 |   2012 |                   2123.46 |                        153 |                578      |                20230   |                    171675   |               1.2704e+07  |
| 13 |   2013 |                   1415.73 |                        147 |                630.649  |                23334   |                    104752   |               7.0184e+06  |
| 14 |   2014 |                   3168.28 |                        102 |               1897.13   |                43634   |                    258781   |               8.0222e+06  |
| 15 |   2015 |                    935.811|                        106 |               1322.92   |                50271   |                     95410.4 |               5.62921e+06 |
| 16 |   2016 |                   2273.93 |                         46 |                247.636  |                 2724   |                    104943   |               1.99391e+06 |


# Assessment of Missingness

## NMAR Analysis

## Missingness Dependency

### 


**Null Hypothesis:** 

**Alternate Hypothesis:** 

### 

**Null Hypothesis:** 

**Alternate Hypothesis:**  

# Hypothesis Testing

**Null Hypothesis:** 

**Alternate Hypothesis:** 

**Test Statistic:** 


# Framing a Prediction Problem


# Baseline Model


# Final Model

# Fairness Analysis

**Null Hypothesis:** 

**Alternative Hypothesis:** 

