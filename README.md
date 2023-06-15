# How Do "Healthy" Recipes Compare to "Unhealthy" Ones, According to Reviewers
UCSD data science DSC80 course analysis project.
---

---
## Abstract
In this analysis, we take a look at the "recipes" and "ratings" datasets from Food.com and, specifically, the recipes that are tagged 'dietary'. We can ask the question of "what makes a recipe more likely to be tagged 'dietary'?"

To answer this, we can build a binary classification. We binarize a column in our dataset indicating whether or not 'dietary' was in the tags of each particular recipe, and use this column as our response variable. We can use the other quantitative values in the "nutrition" column (such as calories, total fat, sugar, etc.) to predict our response variable. 

To evaluate our model, we use accuracy as our metric. 58% of recipes in our dataset is labeled 'dietary', while 42% is not. In other words, our class distribution is relatively balanced. Thus, we are able to use accuracy instead of F-1 score and have a metric that is easier to interpret. 

## Baseline Model 
For our prediction, we built a DecisionTreeClassfication model, that uses four quantitative features: 'total fat','calories','sodium', and 'carbohydrates' values of each recipe as its features. 
After training our model, we get an accuracy of 0.545 on our training set. Our current model is not great: it is decent at predicting from nutritional values whether a recipe is 'dietary' or not; however a large issue with our current model is that the accuracy score of our training data is significantly higher (at 0.994). In other words, the model is overfitted. 

## Final Model 