---
title       : CHAID Analysis - Credit Risk Model
description : We will be using CHAID analysis to classify prospective bank customers into 'Defaults' and 'No Defaults'


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:3cdca1020f
## Categorical or Continuous?

For a classification problems like Credit Risk Model, where we want to profile prospective bank customers into 'Defaults' or 'No Defaults'. What is the nature of the Dependent Variable, also known as the Response variable (Y)?

*** =instructions
- Continuous
- Categorical
- Ratio
- Ordinal

*** =hint
We want to classify the response into one of the two categories: 'Default' or 'No Default'

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

msg_bad <- "That is not correct!"
msg_success <- "Exactly! There seems to be a very bad action movie in the dataset."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:ca4858dfca
## Which algorithm to choose?

For classification problems we generally use:
(a) Logistic Regression Model
(b) Linear Regression Model
(c) Decision Trees Model
(d) K-means Clustering Model

*** =instructions
- (a), (b), and (c)
- (a), (b), and (d)
- Only (a) and (c)
- Only (b) and (d)

*** =hint
Linear Regression model uses a continuous response variable and K-means is generally used for unsupervised machine learning!

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

msg_bad <- "That is not correct!"
msg_success <- "Awesome!."
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:4136bb3dab
## Decision Tree: CHAID

In this exercise, you will be working on `loan_data` dataset. This dataset contains data about a hypothetical bank's customers. In this exercise, we'll have a look at this dataset and continue to work with this in the remainder of the exercises.

A dataset is already available in the workspace, so, you can start playing with it already!

Start by exploring the data at hand. 

*** =instructions
- Print the first 5 rows of `loan_data`.
- Print the last 5 rows of `loan_data`.
- Check out the structure of `loan_data`.
- Use `hist()` function to plot the historgram of `age` variable. Name the graph as "Distribution of Age", the x-axis as "Age" and the y-axis as "Count" 

*** =hint
- Use `head()` for the first instruction.
- Use `tail()` for the second instruction.
- Use `str()` for the third instruction.
- For the plot, use `loan_data$age`, `main =`, `x-lab =`, `y-lab` respectively as the arguments.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

loan_data <- readRDS("https://github.com/gudmhbagde/CHAID-analysis-using-R/blob/master/loan_data_ch1.rds")

```

*** =sample_code
```{r}
# loan_data is available in your workspace
# Print the first 5 lines of loan_data to console


# Print the last 5 lines of loan_data to console


# Look at the structure of loan_data


# Plot histogram of age

```

*** =solution
```{r}
# loan_data is available in your workspace
# Print the first 5 lines of loan_data to console
head(loan_data, 5)

# Print the last 5 lines of loan_data to console
tail(loan_data, 5)

# Look at the structure of loan_data
str(loan_data)

# Plot histogram of age
hist(loan_data$age, main = "Distribution of Age", xlab = "Age", ylab = "Count")
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_function("str", args = "object",
              not_called_msg = "You didn't call `str()`!",
              incorrect_msg = "You didn't call `str(object = ...)` with the correct argument, `object`.")

test_function("hist", args = "x")
test_function("hist", args = "main")
test_function("hist", args = "xlab")
test_function("hist", args = "ylab")

test_error()

success_msg("Good work!")
```
