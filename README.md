# recipes-analysis
dataset analysis for final eecs398 project

# Introduction

TODO: answer prompt and remove it.

PROMPT: Provide an introduction to your dataset, and clearly state the one question your homework is centered around. Why should readers of your website care about the dataset and your question specifically? Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns.


num rows: 234429

# Data Cleaning and Exploratory Data Analysis

TODO: answer the prompt for the first part and remove it. check if embedding of food_cleaned worked. 
TODO: embed the univariate plotly plot. - the boxplots? answer the prompt and remove it 
TODO: embed the bivariate plotly plot and ANSWER THE PROMPTS
- the correlation heatmap
- ratings of 4 and 5 vs all the variables
TODO: embed the table for the aggregate columns and answer the prompts
TODO: imputation lowkey move this up but just answer the prompt, also create the distributions plot! 

PROMPT 1: Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).

## Food Data (Head)

Below is the head of the `food_cleaned` dataframe:

<iframe
  src="assets/food_cleaned_head.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

PROMPT 2: Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present, and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

## Boxplots for Food Data

Below are interactive boxplots for the `food_cleaned` dataset:

<iframe
  src="assets/food_boxplots.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

PROMPT 3: Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

## Correlation Matrix of High-Rated Recipes

Below is an interactive heatmap showing the correlation matrix for recipes with average ratings between 4.5 and 5:

<iframe
  src="assets/high_rated_correlation_heatmap.html"
  width="800"
  height="800"
  frameborder="0"
></iframe>

## Scatter Plot: Avg Rating vs. Minutes

Below is an interactive scatter plot showing the relationship between average ratings and the time in minutes:

<iframe
  src="assets/scatter_avg_rating_vs_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

PROMPT 4: Embed at least one grouped table or pivot table in your website and explain its significance.

PROMPT 5: If you imputed any missing values, visualize the distributions of the imputed columns before and after imputation. Describe which imputation technique you chose to use and why. If you didn’t fill in any missing values, discuss why not.

# Framing a Prediction Problem

# Baseline Model

# Final Model
