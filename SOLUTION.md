# Solution

## Adding batch ID and equipment status

First of all we need to preprocess data in such a way that every record also has the batch ID and equipment status for which it was recorded. Some batches span across several days (in our case for maximum two consecutive days). We will reorganize our data such that all the records corresponding to one batch will be located in the file corresponding to the date when this batch was finished. As a result we will have a list of new files (the same number as it was) but we will know exactly to which batch and status every record belongs to and also we will be able to process files independently:

1. This is very helpful to all future transformations and aggregations.
2. In practice it can be done once for each file in a scheduled manner.

Take a look at the _defining-batch-and-status.ipynb_ to see how this can be done.


## Calculating statistics for every batch and status

This is the point where we start to use pySpark. Take a look at the _calculating-batch-statistics.ipynb_ to see how agregations were made. In the notebook we use both ways of wrangling with the data: SQL based approach and Pandas based approach.

As a result for every batch we have 50 features (5 for every status and equipment), 25 for the POWER sensor and 25 for the TEMP sensor. Final step will be to apply ML to this data to predict target variable.


## Modelling approach

For the modelling part we continue to use pySpark, specifically we use pySpark MLlib. For testing models we use 80%/20% train/test split. We start with a simple Linear Regression and achieve an R-squared of 91%. Next, we try to tune the parameters with the 5-fold cross-validation scheme and we also try a Random Forest model but it performs much worse, so we decided to continue with a linear model.

After looking closer at the features we found that many of them are very correlated and it helped to reduce their number to 10. Only this helped to reduce overfitting and increased test R-squared to 92%.

Feature reduction also allowed us to use polynomial features, in our case we used second-order features. This step significantly improves the model and achieves 97% R-squared.

All these steps can be found in the _building-predictive-models.ipynb_.

