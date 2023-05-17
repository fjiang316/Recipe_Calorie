# An Investigation on the Trend of Recipe's Calories Over Years
**Authors**: Feiyang Jiang, Yiming Jia
## Project Overview
This is a data science project on investigating how people's preference on the calories of the recipe has changed over years. 
The dataset used to investigate the topic can be find [here](https://dsc80.com/project3/recipes-and-ratings/food.com), and is originally scrapped from [this source](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf). This project is for DSC80 at UCSD.

## Investigating Topic and Introduction
Nowadays, with the increase in urbanization, people lives a faster paced lifestyle than before. With more pressure, many studies have shown that people tend to eat high calorie food that secrete more dopamine to gain happiness. This would raise serious health concern since diet with high calories usually causes severe health risk such as high cholesterol, heart disease, diabetes, etc. In order to find out whether people's increasing preference to food with higher calories is true in recent years, we will investigate on the corelation between years and calories of food using the above dataset.

## Introduction to the Datasets in this Study
The first data set we are using contains the recipes information from 2008 to 2018, which has 83,782 rows. The other contains people's opinions to these recipes, which has 731,927 rows.

We used `nutrition` column in the first dataset, it contains the information of calories, total fat, sugar, protein, saturated fat, and carbohydrates. Since we only focus on the calories, we seperated them into different columns and assigned in the data set named `calories (#)`, and only use the calories column for our study.

We used `date` column in the second dataset, it contains information about when people submitted their opinions. We extracted year from the submitted time, and assigned in the dataset named `year`.
