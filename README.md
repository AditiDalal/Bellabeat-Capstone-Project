# Bellabeat - How Can A Wellness Company Play It Smart?
Google Data Analytics Capstone Project by Aditi Dalal

# Background
## About Bellabeat
Bellabeat is a wellness company for women that produces high-tech smart-devices to monitor women's health and fitness. They are a small company but have the potential to become a major player in the smart-device market. Urška Sršen, cofounder and Chief Creative Officer, believes that analyzing past fitness data will help the company find new growth opportunities.

## Major Stakeholders
 - **Urška Sršen:** Bellabeat’s cofounder and Chief Creative Officer
 - **Sando Mur:** Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
 - **Bellabeat marketing analytics team:** A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.

## Business Task
Identify trends in smart device usage using data from FitBit Fitness Tracker App and apply them to Bellabeat customers and products to help influence Bellabeat marketing strategy.

# Preparing the Data
Data was collected from a Public Domain dataset called [Kaggle: Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit). This data contains tracker information from 30 FitBit users and was collected via survey by Amazon Mechanical Turk between 3/12/2016 and 5/12/2016. 

## Limitations
 - Sample size is small (30) and not representative of a large population
 - The data is outdated since it's from 5 years ago, it will not reflect tracker usage by users today
 - Because the data is also from a third-party source, we cannot cite the original data and it's not a primary source

However, the data does match most of the parameters that Bellabeat is looking for.

## Data used
First, I installed the tidyverse, skimrr, janitor, sqldf, and lattice packages and loaded the following files to analyze.

 - `dailyActivity <- read.csv(file = 'FitBitData/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv')`
 - `dailySleep <- read.csv(file = 'FitBitData/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv')`
 - `weightLog <- read.csv(file = 'FitBitData/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv')`

# Process
RStudio was used to process and clean the datasets in order to select the specific data used in the analysis. 

First, I analyzed the datasets to see the limitations of each one.
 - `str_c(n_distinct(dailyActivity$Id), " distinct users in the Daily Activities file")`
 - `str_c(n_distinct(dailySleep$Id), " distinct users in the Sleep Day file")`
 - `str_c(n_distinct(weightLog$Id), " distinct users in the Weight Log file")`
 
 The output is as follows:
  - [1] "33 distinct users in the Daily Activities file"
  - [1] "24 distinct users in the Sleep Day file"
  - [1] "8 distinct users in the Weight Log file"

There is missing data from users in the SleepDay and WeightLog files which are the 2 metrics that must be manually reported by users.

Then I analyzed how many entries there were in each file to find discrepancies.
 - `nrow(dailyActivity)`
 - `nrow(dailySleep)`
 - `nrow(weightLog)`

The output is as follows:
 - 940
 - 413
 - 67

Clearly, there is much less data for the metrics that are manually inputted and the data that is automatically gathered.

To see whether the distribution of manually logged weights was even among the users who did log their weight, I made a pie chart in Google Sheets.
![Percentage of Manual Reports in the WeightLog File by ID](![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/528d0de3-a685-48e4-9d13-1215506c0506/Untitled.png)
