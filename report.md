# [***Work in Progress...***] CASE STUDY: How can a wellness company play it smart? - Bella beat

## **Preamble**
- [Bellabeat](https://bellabeat.com/) is a successful small company in the field of manufacturing high-tech health-focused products for women.
The company CEO believes that analyzing smart device fitness data could help unlock new growth opportunities for the company.
- This case study will follow the basic data analysis steps of Ask, Prepare, Process, Analyze and Share
- *Characters involved:* CEO of bellabeat, client's marketing analytics team
- *Products involved:* Bellabeat app(*app provides users with health data related to their activity, sleep, stress,
menstrual cycle, and mindfulness habits*), Leaf(*wearable smart tracker*), Time(*smartwatch*), Spring (*waterbottle*).

:checkered_flag: **Goal:** Use the smart devices data provided by the client to find useful insights and help guide the marketing team for future plans.

## **ASK**
Need to layout the questions that will/might effect the analysis, key tasks and a deliverable
- Guiding Questions:
  1. What is the problem we are trying to solve
  2. How can the insights drive business decisions
- Key tasks:
  1. Identified the business task / Goal
  2. Identified the key stakeholders / Character involved
- Deliverable:
  - Use the findings from the data to provide conclusions which effect the business in a positive manner. Document the entire proceedings.

## **PREPARE**
Here we collect our dataset to process and check multiple factors affecting that data before we begin to operate on that data. some of the the questions might be

Q: What is the dataset being used?  
A: [Fitbit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) is an open source Kaggle Dataset.

Q: Where is the dataset being stored for processing?  
A: The csv files are loaded into BigQuery tables in the cloud environment.

Q: Are there any bias or issues in credibility of data?  
A: No. The data is collected by [Möbius](https://www.kaggle.com/arashnic) and is under the license [CC0: Public Domain](https://creativecommons.org/publicdomain/zero/1.0/).

Q: How did you verify the data’s integrity?  
A: The data is skimmed to check for any inconsisities or missings. There are no such findings.

## **PROCESS**

+ csv file -> BQ table mapping is as follows:  

| #    | CSV file name | BQ table name  |
|-----:|---------------|----------------|
| 1 | dailyActivity_merged | daily_activity |
| 2 | dailyCalories_merged | daily_calories|
| 3 | dailyIntensities_merged | daily_intensities |
| 4 | dailySteps_merged | daily_steps |
| 5 | heartrate_seconds_merged | heartrate_seconds |
| 6 | seconds_merged | heartrate_seconds |
| 7 | hourlyCalories_merged | hourly_calories |
| 8 | hourlySteps_merged | hourly_steps | 
| 9 | minuteCaloriesNarrow_merged | minute_calories_narrow |
| 10 | minuteCaloriesWide_merged | minute_calories_wide |
| 11 | minuteIntensitiesNarrow_merged | minute_intensities_narrow |
| 12 | minuteIntensitiesWide_merged |   minute_intensities_wide |
| 13 | minuteMETsNarrow_merged | minute_METs_narrow |
| 14 | minuteSleep_merged | minute_sleep |
| 15 | minuteStepsNarrow_merged | minute_steps_narrow |
| 16 | minuteStepsWide_merged | minute_steps_narrow |
| 17 | sleepDay_merged | sleepDay | 
| 18 | weightLogInfo_merged | weight_log_info |

+ Since tables with column data in the format mm/dd/yyyy hh:mm:ss AM/PM are loaded as *string* datatype, need to convert them into *timestamp* datatype. The code snippet to modify the data type is shown below:
```sql
CREATE OR REPLACE TABLE `capstone.heartrate_seconds` AS
SELECT
  * EXCEPT (time),
  PARSE_TIMESTAMP("%m/%d/%Y %I:%M:%S %p", time) time
FROM
  `capstone.heartrate_seconds`
```
+ There are NULL values in the table 'weight_log_info', which are replaced using the expression ```IFNULL(column, value_to_replace)```
