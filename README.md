# An Investigation on the Trend of Recipe's Calories Over Years
**Authors**: Feiyang Jiang, Yiming Jia
## Project Overview
This is a data science project on investigating how people's preference on the calories of the recipe has changed over years. 
The dataset used to investigate the topic can be find [here](https://dsc80.com/project3/recipes-and-ratings/food.com), and is originally scrapped from [this source](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf). This project is for DSC80 at UCSD.

## Investigating Topic and Introduction
Nowadays, with the increase in urbanization, people lives a faster paced lifestyle than before. With more pressure, many studies have shown that people tend to eat high calorie food that secrete more dopamine to gain happiness. This would raise serious health concern since diet with high calories usually causes severe health risk such as high cholesterol, heart disease, diabetes, etc. **In order to find out whether people's increasing preference to food with higher calories is true in recent years, we will investigate on the corelation between years and calories of food using the above dataset**.

### Introduction to the Datasets in this Study
The first data set we are using contains the information of 83782 recipes from 2008 to 2018 on food.com, with 10 columns recording the following information:

|Column	                 |Description|
|---                     |---        |
|`'name'	`            |Recipe name|
|`'id'`	                 |Recipe ID|
|`'minutes'`	         |Minutes to prepare recipe|
|`'contributor_id'`	     |User ID who submitted this recipe|
|`'submitted'`	            | Date recipe was submitted|
|`'tags'`	              |Food.com tags for recipe|
|`'nutrition'`	          |Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein    (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”|
|`'n_steps'`	          |Number of steps in recipe|
|`'steps'`	              |Text for recipe steps, in order|
|`'description'`	     | User-provided description|

The other data set we used contains people's opinions to recipes on food.com, which is consists of 731,927 total reviews. It consists of 5 different columns as listed below:

|Column|Description|
|---|---|
|`'user_id'`	|User ID|
|`'recipe_id'`	|Recipe ID|
|`'date'`	|Date of interaction|
|`'rating'`	|Rating given|
|`'review'`	|Review text|

In our study, we mainly used two columns. We used `nutrition` column in the first dataset, which contains the information of calories, total fat, sugar, protein, saturated fat, and carbohydrates. Since we only focus on the calories, we seperated them into different columns and assigned the portion that indicates calories of each recipe as a new column called `calories (#)` (we will address this column as `calories` for future reference in this study), and only use the calories column for our study.

We used `date` column in the second dataset, it contains information about when people submitted their opinions on the recipes, which is a relatively accurate reflection on people's preference over time. Since we are investigating the trend in terms of years, we extracted year from the `date` column, and assigned this as a new column named `year`.

Note: We also takes into account the `rating` column a several times since that could be an indicator of people's preference on a recipe.


## Cleaning Data and Exploratory Data Analysis
### Data Cleaning
In order to increase the readability and accuracy of our data, we followed the following steps to clean our DataFrame:

1. **Left Merging the recipe and review data sets into one dataframe**: This is for the purpose of matching comments with corresponding recipes to make comparison easier.
2. **Fill the ratings of 0 with N/A**: This step is necessary in cleaning the data because ratings of 0 generally indicates missing values in rating. This is because on most websites with ratings, the lowest rating one can give is always 1, and 0 is not a valid rating, so it's likely an auto-fill on missing rating by the machine.
3. **Convert columns into correct types**: This step is to make the information of each column more accurate, for example, in this study, we convert `date` and `submitted` columns into DateTime type, `minutes` to float type, `n_steps` and `n_ingredients` into integer type.
4. **Extract reviewed year information from `date` column**: As explained in previous part, since we are investigating the trend in terms of years, we extracted year from the `date` column, and assigned this as a new column named `year`.
5. **Categorize different calorie level, assign to each recipe**: Because we noticed the calories of the recipes in our dataframe has a wide range, in order to make the pattern more obvious and not affected by outliers, we categorize calories into 7 categories, being: 0-250 snack, 250-500 breakfast, 500-750 lunch, 750-1000 dinner, 1000-1250 jumbo meal, 1250-1500 family meal, 1500+ holiday meal. This information is stored in a new column named `meal_type`.

After data cleaning, the combined DataFrame looks like the following (only showing the first 5 rows for illustration, the actual combined DataFrame has 234429 records):

|   calories (#) | meal_type   | date                |   year |
|---------------:|:------------|:--------------------|-------:|
|          138.4 | snack       | 2008-11-19 00:00:00 |   2008 |
|          595.1 | lunch       | 2012-01-26 00:00:00 |   2012 |
|          194.8 | snack       | 2008-12-31 00:00:00 |   2008 |
|          194.8 | snack       | 2009-04-13 00:00:00 |   2009 |
|          194.8 | snack       | 2013-08-02 00:00:00 |   2013 |

### Univariate Analysis
With data cleaned for analyzation, we looked at the overall distribution of the calories of the recipes and the distribution of years in which the recipes has been commented on.

<iframe src="https://github.com/fjiang316/Recipe_Calorie/tree/main/figures/year_distri.html" width=800 height=600 frameBorder=0></iframe>
