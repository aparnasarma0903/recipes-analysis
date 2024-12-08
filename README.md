# recipes-analysis
dataset analysis for final eecs398 project

## Introduction

In this project we investigated the recipes and ratings dataset. 
The recipes and ratings dataset comes from food.com and contains columns describing the recipes themselves like ingredients, descriptions, tags, and cook time, as well as ratings and reviews.

Some relationships that I was particularly interested in were:

* which aspects of a recipe contribute to its average rating?
* how does user reviews affect subsequent user reviews?
* what about the ingredients and the health aspects of recipes?

Ultimately, I chose to focus on the complexity of a recipe, as it relates to cook time, number of steps, and number of ingredients, and how that affects the ratings of a recipe.

This dataset is quite large, containing 234429 rows. Some relevant columns we will see in the analysis are:
* n_steps (number of steps, int)
* n_ingredients (number of ingredients, int)
* minutes (how long a recipe takes, int)
* avg_rating (average rating for a recipe, using the ratings column which takes on values [0, 5])
* tags (the tags that user choose to associate their recipe with when submitting, list)

## Data Cleaning and Exploratory Data Analysis
Original Issues:
The dataset, derived from user-submitted recipes and interactions, inherently contained missing values, outliers, and inconsistencies. These issues likely arose from variability in user inputs, irregular data recording, or system constraints.

The data included two separate datasets:  
1. `RAW_recipes.csv` (recipe details)  
2. `RAW_interactions.csv` (user interactions, including ratings) 

These two datasets were merged in order to analyze recipes and their associated user ratings at the same time.

Cleaning Choices:
Cleaning steps were designed to address these issues while preserving the integrity of the data:
* Outliers were managed using domain-specific thresholds (e.g., relaxing minutes bounds), acknowledging the natural variability in cooking times and recipe complexity.
* The only missing values were in the ratings column, which meant that those recipes had no reviews yet. I chose not to impute any of these missing values, since it still makes sense for those missing values to exist. Later, for the prediction portion, these rows were dropped for the purpose of meaningful analysis.

>>One major limitation that may have affected some of the analysis in this report was that the sheer size of the dataset consistently crashed the device. In order to produce meaningful insights, I had to work with a smaller subset of the data for some time until some storage issues were resolved. At first, I was aggressive with removing outliers and exploring smaller subsets of the data to complete the initial exploratory data analysis, and even considered changing the research question multiple times to mitigate this issue. 

Impact on Analyses:
The cleaned dataset enabled robust exploratory and inferential analyses, reducing bias and enhancing the interpretability of patterns, such as the relationship between cooking time and ratings.

Below is the head of the `food_cleaned` dataframe:

<iframe
  src="assets/food_cleaned_head.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
Univariate Analysis:
These boxplots show the distributions of the columns we are looking at to analyze the "complexity" of a review (minutes, n-ingredients, n_steps)
We can see that all the variables take on a large range of values between each of their units, respectively. in addition, there are so many reviews, that the data for n-steps and n_ingredients is pretty evenly distributed across the entire range of values, while minutes is skewed to the right.

<iframe
  src="assets/food_boxplots.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
Bivariate Analysis:

Below is an interactive heatmap showing the correlation matrix for recipes with average ratings between 4.5 and 5:

<iframe
  src="assets/high_rated_correlation_heatmap.html"
  width="800"
  height="800"
  frameborder="0"
></iframe>

I chose to zoom in on the highly rated recipes to see if there were any particular categories that led to a more positive review. Initially, looking at the entire dataset didn't show any clear association between any variables. Even with zooming in, there are no clear associations between avg_ratings and any other categories. The only associations we see are in the columns from the nutrition section, where calories of a recipe is associated positively with carbs and proteins, which makes sense. 

Below is an interactive scatter plot showing the relationship between average ratings and the time in minutes:

<iframe
  src="assets/scatter_avg_rating_vs_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This view shows that majority of the highly rated recipes are between 0 and 50 minutes.

---
<iframe
  src="assets/top_tags_avg_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/bottom_tags_avg_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

These tables highlight the top 10 and bottom 10 tags based on their average ratings. They provide insight into the types of recipes that users rate highly or poorly. For example:  
- **High-rated tags** may indicate popular recipe styles or cuisines.  
- **Low-rated tags** can guide recipe creators to improve content in those categories.  


<iframe
  src="assets/tag_calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This table groups recipes by tags and displays statistics on caloric content (`mean`, `median`, `std`). It helps answer questions like:  
- Which recipe categories tend to have higher caloric content?  
- How variable are the calories within a tag?  

Such insights are valuable for users focusing on nutrition, meal planning, or dietary restrictions.  

<iframe
  src="assets/recipes_over_time.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This visualization aggregates recipes by the year of submission, providing a temporal perspective on recipe trends. It answers questions like:  
- When was the peak of user submissions?  
- Are recipe submissions increasing or decreasing over time?  

Such trends can guide platform strategies and provide historical context to the dataset.  

## Framing a Prediction Problem

## Baseline Model

## Final Model
