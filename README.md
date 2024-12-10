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


### Bivariate Analysis

### Grouping and Aggregates

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

