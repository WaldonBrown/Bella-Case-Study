# Bella-Case-Study
## Exploring a Google Data Analytics Case Study: Strategies for Smart Moves in the Wellness Industry.

## Table of Contents
- [Introduction](#introduction)
- [ASK](#ask)
- [Business Task](#business-task)
- [PREPARE](#prepare)
- [Data Integrity](#data-integrity)
- [PROCESS](#process)
- [Tools](#tools)
- [Cleaning and Processing](#cleaning-and-processing)
- [Conclusion to processing](#conclusion-to-processing)
- [Analyze](#analyze)
- [Insights:](#insights:)
- [Share](#share)
  



### Introduction

Bellabeat stands at the forefront of cutting-edge wellness and fitness technology, with a unique emphasis on women's health and vitality. Offering an array of sleek wearable devices and intuitive mobile apps, Bellabeat empowers users to effortlessly monitor and enhance their well-being. From tracking physical activity and sleep patterns to managing stress levels and reproductive health, Bellabeat's innovative products provide comprehensive insights to guide informed lifestyle choices. Committed to fostering a holistic approach to wellness, Bellabeat seamlessly integrates technology into everyday life while prioritizing user comfort and health.

View  [Bellabeat](https://bellabeat.com/) for more information.

## ASK

During this stage, it's essential to pinpoint the key stakeholders involved. Specifically, in this scenario, we can identify the following stakeholders

- **Urška Sršen:** Co-founder of Bellabeat - Chief Creative Officer - initiated the project.
-  **Sando Mur:** Cofounder of Bellabeat - Mathematician - Member of Bellabeat executive team
-  **Bellabeat marketing analytics team:** Bellabeat's analyst team - Helping to guide the company's marketing strategy  


#### Business Task:

Analyze Fitbit user fitness data to unveil usage trends and provide key marketing recommendations for enhancing growth and digital marketing strategies for Bellabeat's product.

## PREPARE


Sršen suggests utilizing publicly available data that investigates the daily habits of users of smart devices. She points out a specific dataset for us to take a look at:

 [FitBit Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit) On Kaggle (CC0: Public Domain, sourced from Mobius): This dataset features personal fitness tracking data from thirty Fitbit users. It includes minute-level data on physical activity, heart rate, and sleep monitoring, offering insights into daily activity, steps, and heart rate patterns.

#### Data Integrity 

Upon reviewing the data, I found certain limitations:
- The data only used 30 individuals, which is not a big sample size
- I can observe that it's fairly comprehensive since it includes data on physical activity, heart rate, and sleep, which are in line with many of Bellabeat's products as well.
- The data is now 8 years old and may no longer accurately represent the current demographics and behaviors of women today.
- The weight logs only consist of a small number of entries from 8 users, which is less than one-third of the total users surveyed.
- The data has limited citations as it was sourced from a third party.

## PROCESS 


During this phase, I reviewed all 18 files and selected 3 datasets to aid in my analysis. The chosen datasets are as follows:

- dailyActivity_merged.csv
- sleepDay_merged.csv
- weightLoginfo_merged.csv

 ### Tools


I initially loaded all files into Excel to review the provided data for any initial errors that needed cleaning. Subsequently, I employed RStudio for data cleaning, transformation, analysis, and visualization purposes.

**Installing and loading packages and libraries**

        install.packages("tidyverse")
        library("tidyverse")
      
        install.packages("setwd")
        library("lubridate")
        
        install.packages("skimr")
        library("skimr")                             #for summarizing data
        install.packages("dplyr")
        library("dplyr")                             # Using dplyr for grouping and summarizing
        library("ggplot2")                           #for data visualization for a scatter plot 
        
        
        install.packages("janitor")
        library("janitor")
        
        install.packages("plotrix")
        library("plotrix")

*Importing data into RStudio**

In this section, I named the data frames 'daily_activity', 'daily_sleep', and 'weight_log' for easier use during this analysis
       
        library(readr)
        daily_activity <- read_csv("Fitabase Data/dailyActivity_merged.csv")
        library(readr)
        daily_sleep <- read_csv("Fitabase Data/sleepDay_merged.csv")
        library(readr)
        weight_log <- read_csv("Fitabase Data/weightLogInfo_merged.csv")

**Viewing the data**

Review the fundamental details, column headings, summary statistics, data structure, and summary reports to identify any missing values or apparent formatting errors.

        head(daily_activity) # Display first few rows of the data
        head(daily_sleep)
        head(weight_log) 
        
        
        colnames(daily_activity)  #identify all column names
        colnames(daily_sleep)  #identify all column names
        colnames(weight_log)  #identify all column names
        
        summary(daily_activity)   # Summary statistics
        summary(daily_sleep)   # Summary statistics
        summary(weight_log)   # Summary statistics
        
        
        str(daily_activity)   # Structure of the data
        str(daily_sleep)   # Structure of the data
        str(weight_log)   # Structure of the data
        
        skim(daily_activity) # Generate a summary report for the dataset 
        skim(daily_sleep)
        skim(weight_log)

        After executing these lines, I gained the following insights:
        
        - Data types of the columns
        - Name and number of columns 
        - Summary statistics for each column 

### Cleaning and Processing

1. Having examined the datasets and their contents, I observed certain areas in need of cleaning to streamline the process. Notably, there were inconsistencies in the column names, which I promptly rectified. Following this adjustment, I reviewed the resulting changes.

 daily_activity <- clean_names(daily_activity)
         daily_sleep <- clean_names(daily_sleep)
         weight_log <- clean_names(weight_log)
         
         #Double-check the columns are uniformed 
         colnames(daily_activity) 
         colnames(daily_sleep)
         colnames(weight_log)
         
         > colnames(daily_activity) 
          [1] "id"                         "activity_date"              "total_steps"               
          [4] "total_distance"             "tracker_distance"           "logged_activities_distance"
          [7] "very_active_distance"       "moderately_active_distance" "light_active_distance"     
         [10] "sedentary_active_distance"  "very_active_minutes"        "fairly_active_minutes"     
         [13] "lightly_active_minutes"     "sedentary_minutes"          "calories"                  
         > colnames(daily_sleep)
         [1] "id"                   "sleep_day"            "total_sleep_records"  "total_minutes_asleep"
         [5] "total_time_in_bed"   
         > colnames(weight_log)
         [1] "id"               "date"             "weight_kg"        "weight_pounds"    "fat"             
         [6] "bmi"              "is_manual_report" "log_id"           


 2. Rename the date columns in activity_date, sleep_day, and date to the same title to merge later on.
                #Rename the 'date' columns in each set to activity date for uniformity
         
         daily_activity <- daily_activity %>%  rename(activity_date = activity_date)
         daily_sleep <- daily_sleep %>% rename(activity_date = sleep_day)
         weight_log <- weight_log %>% rename(activity_date = date)

   3. Since the ‘’id’’ columns are identifiers in this data set, I do not want them to be seen as a numerical value. Next, I want to convert the ID field to a character data type across all data sets and check the structure after this change. 

          #Convert ID field to a character data type
          daily_activity <- daily_activity %>%  
          mutate_at(vars("id"), as.character)
          
          daily_sleep <- daily_sleep %>%
          mutate_at(vars("id"), as.character)
          
          weight_log <- weight_log %>%
          mutate_at(vars("id"), as.character)
          
          #check the structure 
          str(daily_activity)
          str(daily_sleep)
          str(weight_log)

            > str(daily_activity)
         tibble [940 × 15] (S3: tbl_df/tbl/data.frame)
       
       > str(daily_sleep                             0

 4. Remove the hour/minutes/seconds from the activity_date column in 'daily_sleep' data set and 'weight_log' data set since it will not be needed for this analysis.
          #Convert activity_date to reflect only date and not time in daily_sleep and weight_log
          daily_sleep$activity_date <- sub(" .*", "", daily_sleep$activity_date)
          
          weight_log$activity_date <- sub(" .*", "", weight_log$activity_date)

5. Now that the column activity_date is uniform in each dataset - let's go ahead and convert the format from character to Date format. Let's also doublecheck to make sure it converted correctly.
         
          library(lubridate)                              
          daily_activity$activity_date <- mdy(daily_activity$activity_date)
          daily_sleep$activity_date <- mdy(daily_sleep$activity_date)
          weight_log$activity_date <- mdy(weight_log$activity_date)
          
          #Check our columns to ensure the changes were made correctly
          
          head(daily_activity)
          head(daily_sleep)
          head(weight_log)

 6. In the initial introduction, I was informed this data included only 30 surveys. To confirm this information is correct, I want to count the number of distinct  IDs

    - There are 940 records for the daily_activity data, 413 records for daily_sleep, and 67 records in weight_log
- There were no null values in each data set
- The activity_date column was in character format and converted to date during our cleaning process
- We have converted the ID column to a character format so they are not seen as numerical
- There are 33 IDs in the daily_activity data set which means we did not receive 30 surveys as mentioned but 33
- We further determine there are more IDs in the daily activity data set compared to the daily_sleep data set which has 24 and the 
  weight_log data set which has 8.
  
7. Now, let's identify how many duplicates we have after receiving this information.

           #Identify how many duplicates we have 
           
           sum(duplicated(daily_activity))
           sum(duplicated(daily_sleep))
           sum(duplicated(weight_log))

           > sum(duplicated(daily_activity))
           [1] 0
           > sum(duplicated(daily_sleep))
           [1] 3
           > sum(duplicated(weight_log))
           [1] 0


Review the duplicate entries to verify if they contain identical information in each row. If so, proceed to eliminate the duplicates and ensure the process is executed accurately.

### Review the duplicate entries to confirm if they are actual duplicates with the same entry in every row
           
              library(janitor)
              get_dupes(daily_sleep)

              > get_dupes(daily_sleep)
              No variable names specified - using all columns.

              A tibble: 6 × 6
                id         activity_date total_sleep_records total_minutes_asleep total_time_in_bed dupe_count
                <chr>      <date>                      <dbl>                <dbl>             <dbl>      <int>
              1 4388161847 2016-05-05                      1                  471               495          2
              2 4388161847 2016-05-05                      1                  471               495          2
              3 4702921684 2016-05-07                      1                  520               543          2
              4 4702921684 2016-05-07                      1                  520               543          2
              5 8378563200 2016-04-25                      1                  388               402          2
              6 8378563200 2016-04-25                      1                  388               402          2

  #Remove all confirmed duplicates from daily_sleep
       daily_sleep <- distinct(daily_sleep, .keep_all = TRUE)
       
       #Double-check the duplicates have been removed 
       sum(duplicated(daily_sleep))
                   
        > sum(duplicated(daily_sleep))
        [1] 0

## Conclusion to processing


In this phase, duplicates were identified in each dataset. Upon examining the daily_sleep dataset, it was found to solely contain duplicates. The getdupes() function was employed to confirm their identical nature. Subsequently, 3 identical duplicates were detected and removed using the distinct function.

With this, the documentation for data cleaning and manipulation is completed. We can now transition to the analysis phase!


## Analyze

As per Bella Beat's request, our focus will be on analyzing the daily activities, weight, and sleep patterns of users to uncover potential trends and correlations. Subsequently, leveraging the insights gained from this comprehensive analysis, we aim to provide strategic marketing recommendations to enhance the company's overall marketing approach.

To kick things off, I'd like to consolidate all the data frames to gain a more comprehensive overview of the information at hand.

 #Combine data frames by id and activity_date
         
         combined_data <- daily_sleep %>%
           right_join(daily_activity, by = c("id", "activity_date")) %>%
           left_join(weight_log, by = c("id", "activity_date"))
         
           #View the new data frame
           View(combined_data)

Next, I want to add a weekday column to determine the correlation between the day of the week and various columns. 

#Create a new column for the weekday based on activity_date and view the new column
         combined_data$weekday <- wday(combined_data$activity_date, label = TRUE)
         
         head(combined_data$weekday)
         
         view(combined_data)

Next, we'll proceed to summarize the data and uncover any noteworthy insights.

#Gather statistic summaries to gain insight
         
         combined_data %>%
         select(total_steps, total_distance, very_active_minutes, fairly_active_minutes, lightly_active_minutes, sedentary_minutes)%>%
         summary()
         
         
          total_steps    total_distance   very_active_minutes fairly_active_minutes
          Min.   :    0   Min.   : 0.000   Min.   :  0.00      Min.   :  0.00       
          1st Qu.: 3790   1st Qu.: 2.620   1st Qu.:  0.00      1st Qu.:  0.00       
          Median : 7406   Median : 5.245   Median :  4.00      Median :  6.00       
          Mean   : 7638   Mean   : 5.490   Mean   : 21.16      Mean   : 13.56       
          3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.: 32.00      3rd Qu.: 19.00       
          Max.   :36019   Max.   :28.030   Max.   :210.00      Max.   :143.00       
          lightly_active_minutes sedentary_minutes
          Min.   :  0.0          Min.   :   0.0   
          1st Qu.:127.0          1st Qu.: 729.8   
          Median :199.0          Median :1057.5   
          Mean   :192.8          Mean   : 991.2   
          3rd Qu.:264.0          3rd Qu.:1229.5   
          Max.   :518.0          Max.   :1440.0  

  
**Insights:**

Based on the data extracted, the average total steps recorded are approximately 7,638 steps. While the CDC recommends a daily step count of around 10,000 steps, recent research from both the CDC and NIH suggests that achieving more than 8,000 steps per day can still yield health benefits.

- The average very active minutes are 21.16 minutes while the sedentary minutes are an average of 991.20 minutes or 16.52 hours. Users' average for light activities is 192.8 minutes or 3.2 hours. This means they are spending most of their time doing light activities compared to the other categories. 
     - It is advised that spending more than 7-10 hours sedentary is bad for your health. 
     - It is recommended to do an average of 30 minutes a day of moderate activity and 75 minutes a week of vigorous activity. (CDC 2)


ombined_data %>%
              select( total_minutes_asleep,total_time_in_bed )%>%
              summary()

              total_minutes_asleep total_time_in_bed
               Min.   : 58.0        Min.   : 61.0    
               1st Qu.:361.0        1st Qu.:403.8    
               Median :432.5        Median :463.0    
               Mean   :419.2        Mean   :458.5    
               3rd Qu.:490.0        3rd Qu.:526.0    
               Max.   :796.0        Max.   :961.0    
               NA's   :530          NA's   :530


- On average, users sleep for approximately 419.2 minutes, equivalent to 6.9 hours. This duration falls slightly below the recommended range of 7-10 hours of sleep, as suggested by the NIH.
- On average, users spend approximately 458.5 minutes, or 7.5 hours, in bed. This indicates a difference of around 39 minutes spent, on average, trying to fall asleep

 combined_data %>%
           select( calories, fat, weight_pounds, bmi )%>%
           summary()
         
               calories         fat        weight_pounds        bmi       
          Min.   :   0   Min.   :22.00   Min.   :116.0   Min.   :21.45  
          1st Qu.:1828   1st Qu.:22.75   1st Qu.:135.4   1st Qu.:23.96  
          Median :2134   Median :23.50   Median :137.8   Median :24.39  
          Mean   :2304   Mean   :23.50   Mean   :158.8   Mean   :25.19  
          3rd Qu.:2793   3rd Qu.:24.25   3rd Qu.:187.5   3rd Qu.:25.56  
          Max.   :4900   Max.   :25.00   Max.   :294.3   Max.   :47.54  
                         NA's   :938     NA's   :873     NA's   :873  


- The average user weighs 158.8 pounds and has a BMI of 25.19, which is slightly higher than the average.

  ## Share

  Now that I have completed the analysis, it's time to create the data visualizations.


            #Gather the sum of the very active minutes
            sum(combined_data$very_active_minutes)
            sum(combined_data$fairly_active_minutes)
            sum(combined_data$lightly_active_minutes)
            sum(combined_data$sedentary_minutes)
            
            #Visual for total steps by the day of the week. 
            
            ggplot(combined_data, aes(x = weekday, y = total_steps)) +
            geom_bar(stat = "identity", fill = "#E69F00") +
            labs(title = "Steps Logged by Day in the Week", x = "Day of the Week", y = "Total Steps"

  





