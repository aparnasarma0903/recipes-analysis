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
  height="300"
  frameborder="0"
></iframe>

<iframe
  src="assets/bottom_tags_avg_rating.html"
  width="800"
  height="300"
  frameborder="0"
></iframe>

These tables highlight the top 10 and bottom 10 tags based on their average ratings. They provide insight into the types of recipes that users rate highly or poorly. For example:  
- **High-rated tags** may indicate popular recipe styles or cuisines.  
- **Low-rated tags** can guide recipe creators to improve content in those categories.  


<iframe
  src="assets/tag_calories.html"
  width="800"
  height="300"
  frameborder="0"
></iframe>

This table groups recipes by tags and displays statistics on caloric content (`mean`, `median`, `std`). It helps answer questions like:  
- Which recipe categories tend to have higher caloric content?  
- How variable are the calories within a tag?  

Such insights are valuable for users focusing on nutrition, meal planning, or dietary restrictions.  

<iframe
  src="assets/recipes_over_time.html"
  width="800"
  height="300"
  frameborder="0"
></iframe>

This visualization aggregates recipes by the year of submission, providing a temporal perspective on recipe trends. It answers questions like:  
- When was the peak of user submissions?  
- Are recipe submissions increasing or decreasing over time?  

Such trends can guide platform strategies and provide historical context to the dataset.  

## Framing a Prediction Problem

**Problem Statement:**  
The goal of this project is to predict the **average rating** (`avg_rating`) of recipes based on certain features such as preparation time (`minutes`) and recipe tags (`tags`). This is a **regression problem** since we are predicting a continuous variable (the average rating) that ranges from 1 to 5.

**Response Variable:**  
The response variable we are predicting is the **`avg_rating`** of each recipe. We chose this as the target variable because understanding the factors influencing recipe ratings can help us identify trends, optimize recipe recommendations, and improve user engagement.

**Model Type:**  
Given that the target variable is continuous, we are performing **regression** rather than classification. The model we are using is a **Linear Regression** model, which predicts the target variable based on a linear combination of the input features.

**Evaluation Metric:**  
We are using **Root Mean Squared Error (RMSE)** as the metric to evaluate the model's performance. RMSE is a popular metric for regression problems because it penalizes large errors more heavily due to its squared nature. This makes it particularly useful for identifying how well the model performs across all the predicted ratings, particularly in cases where small errors are tolerable but large errors are not.

We chose RMSE over other metrics like Mean Absolute Error (MAE) because RMSE is more sensitive to outliers, which is important in this context since extreme ratings (e.g., 1 or 5) might indicate significant user dissatisfaction or satisfaction. Therefore, we want to ensure that our model performs well across the entire range of ratings.

**Time of Prediction:**  
At the time of prediction, we would only have access to the **preparation time (`minutes`)** and **recipe tags (`tags`)** as input features. These are the features available before the recipe is rated, as the rating is a result of the user’s feedback after the recipe is consumed. Therefore, no future data or post-rating information should be used during model training or prediction.

### Model Pipeline  

The pipeline used for this analysis preprocesses the features and applies the Linear Regression model as follows:
1. **Preprocessing:**  
   - `minutes` is treated as a numerical feature.
   - `tags` are transformed into binary columns (one for each possible tag) using the **MultiLabelBinarizer**.

2. **Model:**  
   - A **Linear Regression** model is then applied to predict the `avg_rating`.

**Cross-validation:**  
We also perform **5-fold cross-validation** to ensure the model's robustness and reduce the potential for overfitting. The **negative mean squared error** is used as the scoring metric for cross-validation.

The evaluation metrics obtained from the model show the RMSE and cross-validation results, which indicate how well the model is generalizing to unseen data.

## Baseline Model

In this analysis, we built a **Linear Regression** model to predict the **average rating** (`avg_rating`) of recipes. The model uses two features:
1. **`minutes`** (quantitative): This feature represents the preparation time of the recipe in minutes.
2. **`tags`** (nominal): This feature is a list of tags associated with each recipe, such as "vegetarian," "gluten-free," etc.

For the **`tags`** feature, which is nominal, we used the **MultiLabelBinarizer** to perform encoding. This process converts each unique tag into a separate binary column, where each column represents the presence or absence of a particular tag in the recipe. For example, if a recipe is tagged with "vegetarian" and "gluten-free," the corresponding columns for these tags will be set to 1, while all other tag columns will be set to 0.

The **`minutes`** feature was treated as a **quantitative** feature, and no additional encoding was necessary since it is already in a numerical format.

### Model Performance

The baseline model was evaluated using the Root Mean Squared Error (RMSE) and cross-validation Mean Squared Error (MSE). The performance metrics are as follows:
- **Baseline Model RMSE: 0.50**
- **Cross-validation MSE: 0.2406**

The RMSE of 0.50 indicates that, on average, the predicted ratings are off by 0.5 points, which suggests that the model performs relatively well in predicting the ratings, given the scale of ratings (1 to 5). 

The cross-validation MSE is 0.2406, which indicates that the model's performance is stable across different subsets of the data and that the errors in the predictions are consistently small.

### Model Evaluation and Conclusion

Overall, the model appears to perform well with an RMSE of 0.50. However, there are still opportunities for improvement. Since linear regression is a simple model, it might not capture all of the complex relationships between the features and the target variable. We could explore more sophisticated models, such as decision trees or random forests, which might better capture non-linear patterns in the data.

At this stage, I believe the model is **good enough** for a baseline, but there is room to explore more advanced modeling techniques to further reduce the error. The cross-validation scores indicate that the model generalizes reasonably well, but fine-tuning the model or using additional features could improve the predictions further.


## Final Model
