# Suicide Rate Analysis (1995-2016)
  
## Project Overview
Hello there! This project is all about the analysis of Suicide Rate Trend from 1995 to 2016.

## Author
* [rfhtorres28](https://github.com/rfhtorres28)

## Table of Contents
* [Methods](#Methods)
* [Data_Cleaning_Process](#Data_Cleaning_Process)
* [Exploratory_Data_Analysis](#Exploratory_Data_Analysis)
* [Overview_of_the_Results](#Overview_of_the_Results)
* [Insights](#Insights)

## Data Source
* The CSV files were downloaded from kaggle website. I created a database on SQL Server and make a table for the cleaned data after Data Cleaning from Python. 

## Tools 
* Python - Data Cleaning/ Exploratory Data Analysis/ Data Visualization

  
## Methods
In this project, I solely used Python for all Data Analysis process from Loading the CSV dataset file to Data Visualization. I used Matplotlib and Seaborn python libraries for Data Visualization.

### First Phase 
 In the initial phase of data preparation, the following tasks were performed: 

 1. Load the CSV file using pd.read_csv().
 3. Removed duplicate rows, change the format of column names, detecting and replacing outliers and filling missing values.
 4. Check if each column has correct data types.

### Second Phase
 EDA involves exploring the sales data to answer some of the following questions:

 1. What year has the most number of suicides? 
 2. Which gender has the most suicide cases?
 3. Which country has the highest suicide case?
 4. Is there are relationship between the measure of standard of living like HDI and GDP to a person commiting a suicide?

### Last Phase
  For each questions, I used necessary python syntax like group by, sorting and other aggregation methods to come up with a table to make an inference. Then I used Matplotlib and Seaborn
  libraries to graph the data.

## Data_Cleaning_Process 
Notebook file for Data Cleaning is uploaded in the repository section. 
https://github.com/rfhtorres28/suicide_rate_analysis_using_python/blob/main/Suicide_Rate_Analysis.ipynb
Steps: 

1. Load the CSV file using pd.read_csv()
2. Remove unecessary characters on column title and capitalize each word.
3. Repeat step 2 for each category columns.
4. Select necessary columns for analysis.
5. Drop duplicate rows
6. Detect outliers by graphing each numeric column using boxplot.
7. If the graph is skewed distribution and has potential outliers, use inter quartile rule for removing outliers.
8. If the graph is symmetric distribution, use standard deviation rule for removing outliers.
9. If the outliers are valid values for each column, just retain them.
10. If the count of outliers are almost 10% of the total values on that column, retain them. Removing the outliers or replacing them can affect the accuracy of the result since 10% is significant.
11. Detect null values and replace them with zero or median value.
12. If the numeric column has null values with extreme outliers, use the median value as a replacement for null values instead of mean value since it can be affected due to presence of extreme outliers.
13. Check if each columns has correct data types.

## Exploratory_Data_Analysis
See the notebook file for EDA process

## Overview_of_the_Results

![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/ed7a568f-caa1-4c6e-8b0e-127441235df6)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/d59aab4d-edd7-4ca6-989c-79ebc61c1522)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/2370a92d-82c7-4557-92a4-fc8141e14d49)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/03b9269a-835f-4712-bbed-0fdb64bc1f6e)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/582f420d-1fc0-4682-80d1-68e46079eb30)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/9a2cc2fc-94f2-4d7f-a5a7-37145d9bfcda)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/b5836624-5bcb-4ad0-8e4b-068bfbe15973)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/9679bcab-1a78-4504-85ab-504d51c2e7c0)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/e4103260-7b55-4bf8-86dd-f48e36afc160)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/972d7436-1c84-4dd9-ada5-533452c02bde)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/f8af108a-20f9-4e88-8a8a-b3c77e814a3e)
![fd13efed-4a71-48c1-a328-365ef726f5bb](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/163068b3-a612-4fd3-9328-325f43aa9a0d)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/dbe0b411-c569-4093-b926-1f8004c1e997)
![image](https://github.com/rfhtorres28/suicide_rate_analysis_using_python/assets/153373159/a6c8b9c2-4e88-4833-95ab-b8147379add7)




## Insights

From 1985 to 2016, there is a decrease trend in suicide cases. The highest suicide cases was on 1999. For suicide number per 100k population, year 1995 was the highest ratio. Russian Federation was the country with the highest suicide cases recorded. Male gender with age bracket of 35-54 years was its dominant charateristics. There was no relationship among HDI, GDP, GDP Per Capita and Suicide Number meaning the measure of standard of living of a country has nothing to do with the person commiting a suicide. 
