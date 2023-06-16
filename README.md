# How Do "Healthy" Recipes Compare to "Unhealthy" Ones, According to Reviewers
UCSD data science DSC80 course analysis project.
---

---
## Abstract
In this analysis, we take a look at the "recipes" and "ratings" datasets from Food.com and, specifically, the recipes that are tagged 'dietary'. We can ask the question of "what makes a recipe more likely to be tagged 'dietary'?"

To answer this, we can build a binary classification. We create a column in our dataset indicating whether or not 'dietary' was in the tags of each particular recipe, and use this column as our response variable. We can use the other quantitative values in the "nutrition" column (such as calories, total fat, sugar, etc.) to predict our response variable. 

To evaluate our model, we use accuracy as our metric. 58% of recipes in our dataset is labeled 'dietary', while 42% is not. In other words, our class distribution is relatively balanced. Thus, we are able to use accuracy instead of F-1 score and have a metric that is easier to interpret. 

## Baseline Model 
For our prediction, we built a DecisionTreeClassfication model, that uses four quantitative features: 'total fat','calories','sodium', and 'carbohydrates' values of each recipe as its features. For our response variable, we one-hot encode using the 'tags' column. 

After training our model, we get an accuracy of 0.545 on our training set. Our current model is not great: it is decent at predicting from nutritional values whether a recipe is 'dietary' or not; however a large issue with our current model is that the accuracy score of our training data is significantly higher (at 0.994). In other words, the model is overfitted. 

## Final Model 

To improve upon the baseline DecisionTree model, we engineer these new features: 

* standardized 'calories' feature
    - Taking a look at the average 'calories' value for dietary and non-dietary recipes, respectively, we notice both groups have some outlier values that may hinder our model accuracy. By re-scaling with standardization, the prediction process is more robust to outliers.
* binarized 'sodium' feature with a threshold of 10
    - The 'dietary' group has an average 'sodium' value that is much smaller than that of the entire population, specifically that it has a mean value of 10. So, indicating 1 of recipes with sodium values higher than 10 and 0 of recipes with sodium values less than 0, aids in our precision value.
* binarized 'calories' feature with a threshold of 450
    - The 'dietary' group has an average 'calories' value that is much smaller than that of the entire population, specifically that it has a mean value of 450. So, indicating 1 of recipes with calorie values higher than 450 and 0 of recipes with calorie values less than 450, aids in our precision value.

To improve upon the baseline DecisionTree model, we also utilize GridSearchCV to tune the tree's hyperparameters. 

    1. Max_depth : not specified in our baseline Decision Tree, so the nodes will  expand until all leaf nodes are pure or until all leaf nodes contain less than min_samples_split. Thus tuning this parameter will prevent overfitting.
    2. min_samples_split : tuned also as a method of preventing overfitting.
    3. criterion : the choices "gini" and "entropy" use different metrics to measure the probability of random misclassification.

After running GridSearchCV with k=5, we find that the tree with hyperparameters criterion='entropy', max_depth=7, min_samples_split=200 performed the best. We then use this tuned DecisionTree to train our dataset from the baseline step.

Our final model achieved an accuracy of 0.606, compared to our baseline model accuracy of 0.545. In addition, the training and testing accuracies of the final model is much more balanced (0.609 on training) indicating that we resolved much of our original overfitting issue.

## Fairness Analysis

We evaluate if our model is fair for two groups:
    "high calorie" : recipes with calorie values greater than 500
    "low calorie" : recipes with calorie values less than 500
To determine if our model performs better for low calorie recipes, we run a permutation test, using precision as the evaluation metric, and the difference in precision as our test statistic. 

Null Hypothesis: 

    Our model is fair. Its precision for high-calorie recipes and low-calorie recipes are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: 

    Our model is unfair. Its precision for low-calorie recipes is higher than its precision for high-calorie recipes.


It seems like the difference in precision across the two groups is not significant, using significance level of 0.05. We achieve a p-value in our permutation test of 
0.224. As this is above our cut-off value, we fail to reject the null hypothesis. Thus, we can infer from our test that our model is fair and performs roughly the same for both low-calorie recipes and high-calorie recipes.
