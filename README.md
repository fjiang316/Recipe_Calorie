# An Investigation on the Trend of Recipe's Calories Over Years
**Authors**: Feiyang Jiang, Yiming Jia
## Project Overview
This is a data science project on investigating how people's preference on the calories of the recipe has changed over years. 
The dataset used to investigate the topic can be find [here](https://dsc80.com/project3/recipes-and-ratings/food.com), and is originally scrapped from [this source](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf). This project is for DSC80 at UCSD.

---
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

---
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
#### Distribution of Years
Here is a histogram on the distribution of the the years when recipes are commented in our data set. We notice it has a right-skewed shape, with more comments being posted in earlier years than in recent years. This observation reminds us that in order to have a fair investigation on people's preference over years, we cannot compare based on the number of comments. Instead, we need to compare based on the proportion of comments on recipes in different calorie categories in each year to make fair comparison.

<iframe src="figures/year_distri.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Calories
Here is a histogram on the distribution of the calories of all recipes in our data set. For the purpose of a more obvious observation on the pattern of the distribution, we zoomed in by selecting only a portion of the graph (calories from 0 to 4000), due to some extreme outliers. Even without the outliers in sight, there's a right-skewed pattern as well. This indicates that in order to not let the outliers affect the later observation of the correlation statistics, it would be better that we divide calories into categories and do analysis on the categorical distribution in each year for fair comparison.

<iframe src="figures/calorie_distri.html" width=800 height=600 frameBorder=0></iframe>


### Bivariate Analysis
To understand whether a correlation exist between two variables, it is also important to look at the relationship between the two variable. In this part, we will use a boxplot to illustrate the relationship between the calories of the recipes and people's preference over years (the proportion of counts for different calories levels in each years).

<iframe src="figures/year_calorie_box.html" width=800 height=600 frameBorder=0></iframe>

From the box plot above, if we look at the two lines indicating the 3rd quatile and the maximun, we notice a steady increasing trend in the level of calorie in recipes people reviewed over years. This tendency seems to suggest that there is a correlation between the years and the calories of food, which indicates people have this trend of favoring food with higher calories over year in our data set.


### Interesting Aggregates
So far, we have only use visual observation on plots to analyze the relationship between year and calories. In order to have a more reliable analysis, we constructed a pivot table that shows the distribution of conditional relative frequency of `meal_type` conditioned on every year. (Recall the `meal_type` column from part of cleaning the data)

A sample pivot table is show below. According to pivot table, there's a gradual increase in the reviews on the proportion of high-calorie recipes in recent years and a decrease in the proportion of low-calorie recipes in recent years. This statistical observation agrees with our previous observation on the box plot.

| meal_type    |     2008.0 |     2009.0 |    2010.0 |    2011.0 |    2012.0 |    2013.0 |    2014.0 |    2015.0 |    2016.0 |    2017.0 |    2018.0 |
|:-------------|-----------:|-----------:|----------:|----------:|----------:|----------:|----------:|----------:|----------:|----------:|----------:|
| snack        | 0.419618   | 0.411173   | 0.434028  | 0.422243  | 0.405003  | 0.385073  | 0.381717  | 0.364489  | 0.370481  | 0.383044  | 0.382773  |
| breakfast    | 0.357484   | 0.349112   | 0.344009  | 0.353171  | 0.347421  | 0.351531  | 0.358347  | 0.344017  | 0.347993  | 0.343531  | 0.326648  |
| lunch        | 0.135695   | 0.135584   | 0.126102  | 0.124747  | 0.139967  | 0.142635  | 0.130763  | 0.139166  | 0.136765  | 0.134455  | 0.134757  |
| dinner       | 0.0458329  | 0.0541099  | 0.0489045 | 0.0491315 | 0.058631  | 0.0600833 | 0.0647059 | 0.0708019 | 0.0649675 | 0.0591556 | 0.0688212 |
| family meal  | 0.00867281 | 0.00912846 | 0.0101975 | 0.0104499 | 0.0101565 | 0.0128586 | 0.0116057 | 0.0182793 | 0.015992  | 0.0142244 | 0.0135623 |
| jumbo meal   | 0.0134523  | 0.0184634  | 0.0156884 | 0.0152808 | 0.0153607 | 0.0199057 | 0.0251192 | 0.0279064 | 0.025987  | 0.0258523 | 0.0298658 |
| holiday meal | 0.0192448  | 0.0224287  | 0.0210711 | 0.0249769 | 0.0234608 | 0.0279138 | 0.0277424 | 0.03534   | 0.0378144 | 0.0397381 | 0.0435724 |

To get a visual representation of this statistics, we have converted it into the below line plot. There's an observable shift in the slope of the lines for each year that "flattened" over years. This indicates the increase in proportion of high calorie recipes in recent years.

<iframe src="figures/pivot_line.html" width=800 height=600 frameBorder=0></iframe>

---
## Assessment of Missingness
### NMAR Analysis
One of the column in our dataset with missing values that is possibly NMAR is the `review` column. The reason for its missingness is most likely due to the reviewer's personal opinion to the recipes itself and not depend on other columns in our data set. People tend to write a lot in review to complain if they dislike the recipe, or they might write a lot if they really enjoy the recipe, but they might not write any review if they feel nothing special to the recipes and thus don't have much to comment on. The possibility that the missingness of review is because people don't feel any special things to leave a review on makes its missingness NMAR.

However, if we can generate another column that indicates people's satisfactory with either thumbs up, thumbs down, or somewhere in middle, there might be some dependency between this new column and the missingness of review, thus making its missingness MAR, because it is highly possible that people who select somewhere in middle for satisfactory are the one who do not submit a review.

### Missingness Dependency
Since we are interested in people's preference of calaories' level over time, and we believe that their preference can be expressed through their rating of recipe, we decide to examine the `rating` column as well here.

We noticed that there are a lot of missing values in the rating column, we wanted to know whether the missingness of rating depends on other columns or not.

We constructed permutation tests to determine the relationship. We have decided to test the dependency between the missingness of `rating` with two columns: `minutes` and `calories (#)`.

#### 1. Rating and Minutes (MCAR)
Null Hypothesis: The missingness of rating does not depend on minutes

Alternative Hypothesis: The missingness of rating depend on minutes

We created a new column indicating the missingness status of the `rating`, and shuffled this column for our permutation. Since `minutes` is numeric, we used the absolute mean difference of `minutes` when `rating` is and is not missing as our test statistics.

Below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.

<iframe src="figures/empi_dist_minute_rate.html" width=800 height=600 frameBorder=0></iframe>

From the graph above and out permutation test, we get a p-value that's greater than our significance level of 5%. So we fail to reject the null hypothesis.

**Therefore, we conclude that it is highly possible that the missingness of `rating` *<u>does not</u>* depend on `minutes` column.**

#### 2. Rating and Calories (MAR)

Null Hypothesis: The missingness of rating does not depend on calories (#)

Alternative Hypothesis: The missingness of rating depend on calories (#)

Similar to the first test with minutes, we created a new column indicating the missingness of `rating` for each row record, and shuffled this column for permutation. Likewise, because `calories` is a numerical variable, we use the absolute mean difference of `calories` when `rating` is and is not missing as our test statistics.

The plot below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.

<iframe src="figures/empi_dist_cal_rate.html" width=800 height=600 frameBorder=0></iframe>

From this graph and our test, we get a p-value that's significantly less than our significance level of 5%. So we reject the null hypothesis.

**Hence, we conclude that the missingness of `rating` *<u>depends</u>* on the `calories` column.**

---
## Hypothesis Testing
Going back to our investigation topic, we are investigating whether it is true that there is an increasing trend of preference to food with higher calories in recent years of the last decade. So far, we have already seen an increasing trend in the usage of proportion of recipes with high calories from 2008 to 2018, but observation alone cannot be a good indicator as to whether this trend shows actual change in people's preference or is the trend merely coincidental. We determined that a good way to test the truthfulness of the trend is to take the distribution at the start and end of the decade for comparison. Hence, we are going to conduct a permutation test on the distribution of people's comment on food with different calorie levels in 2008 and the distribution in 2018 to see whether there is an actual increase in preference to higher calorie food or not.

**Null Hypothesis**: the calories of recipes people commented on in 2008 and 2018 comes from the same population. (The observation is merely coincidental, there's no actual trend proving a shift in preference.)

**Alternative Hypothesis**: the recipes people use in 2018 has higher calories than recipes people use in 2008. (The observation is not coincidental, there's an actual trend proving a shift in preference.)

**Test Statistics**: For the purpose of not letting numeric outliers impact the observational statistics, in our test, we are going to use `meal_type` to categorize different calorie levels and use this column as our variable. Because `meal_type` is categorical, in order to better compare its distribution in the two different years, we are going to use <u>Total Varience Distribution (TVD)</u> as our test statistics.

**Significance Level**: To ensure the accuracy of our conclusion on the existence of changes in preference, we determined to use a significance level of 5% to increase accuracy of our randomized test result.

The plot below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.

<iframe src="figures/permutation_test_dist.html" width=800 height=600 frameBorder=0></iframe>

From the figure above, we see that the change in distribution we get from our observed statistics is not likely coincidental, and this is also supported by our p-value from the test that is less than our significance level. Hence, we **reject our null hypothesis**.

## Conclusion:
From the test conducted above, we conclude that our observed data in the dataset shows strong evidence against our null hypothesis that the difference in distribution of calorie preference in the two years are merely coincidental. Hence, we reject that there isn't an increase in people's preference to food with higher calories over the decade from 2008 to 2018. Although we cannot conclude the reason for this trend, the changes in the preference is highly likely a real trend over the last decade.