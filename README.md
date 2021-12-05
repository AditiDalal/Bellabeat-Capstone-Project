# Bellabeat - How Can A Wellness Company Play It Smart?
Google Data Analytics Capstone Project by Aditi Dalal

# Background
## About Bellabeat
Bellabeat is a wellness company that produces high-tech smart-devices to monitor women's health and fitness. Although they are a small company, they have the potential to become a major player in the smart-device market.

## Major Stakeholders
 - **Urška Sršen:** Bellabeat’s cofounder and Chief Creative Officer
 - **Sando Mur:** Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
 - **Bellabeat marketing analytics team:** A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.

## Business Task
Identify trends in smart device usage using data from FitBit Fitness Tracker App and apply them to Bellabeat customers and products to help influence Bellabeat marketing strategy.

# Preparing the Data
Data was collected from a Public Domain dataset called [Kaggle: Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit). This dataset contains tracker information from 30 FitBit users and was collected via survey by Amazon Mechanical Turk in the span of 31 days from 3/12/2016 to 5/12/2016. 

## Limitations
 - Sample size is small (30) and not representative of a large population
 - The data is outdated, since it's from 5 years ago, and will not reflect tracker usage by users today
 - Because the data is also from a third-party source, we cannot cite the original data and it's not original

However, the data does match most of the parameters that Bellabeat is looking for, such as steps, calories, activity time, weight, and sleep.

## Data used
First, I installed the tidyverse, skimr, janitor, sqldf, and lattice packages and loaded the following files to analyze.

 - `dailyActivity <- read.csv(file = 'FitBitData/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv')` 
 - `str_c(n_distinct(dailySleep$Id), " distinct users in the Sleep Day file")`
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

Clearly, there is much less data for the metrics that are manually inputted compared to the data that is automatically gathered.

# Analysis
The dailyActivity file contains the following information: Id, ActivityDate, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, and Calories. For the purposes of my analysis, I will be using the Id, TotalSteps, TotalDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, and Calories information. The weightLog file contains Id, Date, Time, AM/PM, WeightKg, WeightPounds, Fat, BMI, IsManualReport, and LogId columns. For my analysis I will use the Id and IsManualReport columns.  Each of the files were copied prior to any changes. 

#### Users Manually Log Their Weight
To see whether the distribution of manually logged weights was even among the users who did log their weight, I made a pie chart in Google Sheets.

![Percentage of Manual Reports in the WeightLog File by ID](https://user-images.githubusercontent.com/95492345/144663466-1978480e-4fce-4337-a2b7-980500dbca9a.png)

In the pie chart above, over 80% of all manual weight logs are given by just ***2 users***. This clearly shows that very few users will actually use the manually logged features rather than the automatically generated data. Users rely on the products to be as hands-free as possible, therefore, my **recommendation** to Bellabeat is to make as many aspects of their products and app automatically recorded as possible and _not_ to market their app as something completely customizable though users have to manually input information.

#### Steps and Calories
One of the biggest measures that users look at is how many steps they walk and how many calories they burn. Therefore, I've analyzed users habits and how many calories they burned. Below is a plot showing how many steps users took and how many calories they burned. They are colored by the total distance they traveled as well.

![Total Steps and Calories, shaded by Total Distance](https://user-images.githubusercontent.com/95492345/144665791-b75d4c9c-a811-47fb-bb5b-9108912f0b17.png)

Here, it's clear that higher steps doesn't necessarily mean that that user burned more calories. Also, in the areas where Total Steps are low but Calories are high, some of those points are dark blue, indicating that their total distance wasn't very high either. Thus, my **recommendation** is that Bellabeat markets their products for a wide audience and show people that they don't need to be fitness buffs to get a good workout and burn calories. 

Another analysis that I've done to truly find out who is using these smart devices is a plot of Average Steps and Average Distance.

![Average Steps vs Average Distance](https://user-images.githubusercontent.com/95492345/144666239-e76db46a-f533-47d4-ae37-afbdbaa3b8dd.png)

The plot above shows a wide range of users and a clear correlation between how many steps users took on average and how far they went. The very wide range of types of users further supports my recommendation that products should be marketed towards everyone and emphasize that you don't have to be a fitness buff or have alredy started your fitness journey. 

#### Intensities and Calories
Besides just analyzing how steps relate to calories, it's important to consider how intense these users workout. A shorter workout that is also more intense is still an indication of who these users are. Below are scatterplots of various intensities and how many calories were burned and are shaded by how many minutes each user worked out for.

![Very Active Distance vs Calories](https://user-images.githubusercontent.com/95492345/144668521-48a8631f-e4bc-48cb-9c7b-39be1ffb832f.png)

As you can see, these data are clustered around lower distances, but also have a wide range of calories. However, the color is not evenly distributed, and many of the lighter blue points (longer times) are clustered around lower distances _and_ lower calories. These users are working out for a very long time yet not burning as many calories as they might expect. Although there are 2 outliers, a good proportion of the data follow the trend of low but very active distances, low calories burned, and a lot of time spent on  the workout. 

![Moderately Active Distance vs Calories](https://user-images.githubusercontent.com/95492345/144668548-19ce4373-f632-468e-ac53-dc32222be3dc.png)

The distribution of points in this plot are centered in lower distances but, again, the calories are widely distributed. The colors range largely in the higher values yet many of these points burn lower calories than expected despite the amount of time spent just as in the previous scatterplot. It's again likely, that only some of the users are maximizing their workouts. Ideally, there would be a positive, linear correlation between ActiveDistance and Calories Burned with lighter colors being in the high-distance, and high-calories burned area. There is also little to no correlation between the two variables graphed, and if there were a line of best fit, it would be almost vertical without the 1 outlier. Thus, users show that calories burned and moderately active distance are mostly independent.

![Lightly Active Distance vs Calories](https://user-images.githubusercontent.com/95492345/144668587-3eb38b74-0a14-47a4-bc25-775a6a1ce6c5.png)

These data are much more spread out than the other 2 scatterplots above. There is a slightly positive linear correlation between the variables. Also, the colors are more distributed towards what should be predicted (lighter colors appear where there were higher distances and higher calories burned). Overall, these lightly active workouts are more effective in following the expected trend.

I propose that the Bellabeat app offer insights into users' individual activity and provide tips and recommendations about how users can improve their workouts. Also, having users fill out a quick survey upon creating an account in the app and then using that data to give them workouts and goals will help users maintain progress and watch their workout difficulties progress just as they do.

# Summary
Here are a few of my suggestions that the marketing team do:
 - Make as little of the data manually logged
 - Market their products for a wide audience and emphasize that users don't need to be fitness buffs and absolute beginners will also benefit
 - Offer insights into personal progress and take a quick survey to create ways for users to have personalized workouts and suggestions.
    - It might also be helpful to show users a summary of their workout with the amount of time spent, intensity level, and calories burned. From there, using certain                 parameters, tell users how to further maximize their workouts.

