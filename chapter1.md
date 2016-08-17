---
title       : CHAID Analysis - Credit Risk Model
description : We will be using CHAID analysis to classify prospective bank customers into 'Defaults' and 'No Defaults'


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:3cdca1020f
## Categorical or Continuous?

For a classification problem like Credit Risk Model, where our objective is to profile prospective bank customers into 'Defaults' or 'No Defaults'. What is the nature of the Dependent Variable, which is also known as the Response variable (Y)?

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
msg_success <- "Awesome!"
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
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:4136bb3dab
## Decision Tree: CHAID

In this exercise, you will be working on `loan_data` dataset. This dataset contains data about a hypothetical bank's customers. In this exercise, we'll have a look at this dataset and continue to work with this in the remainder of the exercises.

The dataset is already available in the workspace, so, you can start playing with it already!

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

loan_data <- read.csv("http://assets.datacamp.com/production/1608/loan_data.csv", stringsAsFactors = FALSE)

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

test_function("head", args = "n",
              not_called_msg = "You didn't call `head()`!",
              incorrect_msg = "You didn't call `head(.., n = ...)` with the correct argument, `n`.")
              
test_function("tail", args = "n",
              not_called_msg = "You didn't call `tail()`!",
              incorrect_msg = "You didn't call `tail(.., n = ...)` with the correct argument, `n`.")
              
test_function("hist", args = "x",
              not_called_msg = "You didn't call `hist()`!",
              incorrect_msg = "You didn't call `hist(x =)` with the correct argument, `x`.")
              
test_function("hist", args = "main",
              incorrect_msg = "You didn't call `hist(.., main =)` with the correct argument, `main`.")
              
test_function("hist", args = "xlab",
              incorrect_msg = "You didn't call `hist(.., xlab =)` with the correct argument, `xlab`.")
              
test_function("hist", args = "ylab",
              incorrect_msg = "You didn't call `hist(.., ylab =)` with the correct argument, `ylab`.")

test_error()

success_msg("Good work!")
```
--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:c1bff77acc
## Distribution of Age?

Is the `age` variable skewed? If yes, is it right skewed or left skewed? The `loan_data` is available in your workspace, if you want to revisit the distribution of `age` variable.

*** =instructions
- No skewness
- Left skewed
- Right skewed

*** =hint
On which side do we see the tail of the curve extend to?

*** =pre_exercise_code
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer

loan_data <- read.csv("http://assets.datacamp.com/production/1608/loan_data.csv", stringsAsFactors = FALSE)
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

msg_bad <- "That is not correct!"
msg_success <- "That is correct! Apart from skewness there is also a person with age of 144 years! Do you think this is plausible?"
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:f220559b36
## Data Cleaning (1)

A person with `age` 144 is not possible. Either this age was entered due to a typo or deliberately to highlight it as an outlier. In either case, we can safely remove this observation from our dataset.

Look again at the `str()` of `load_data` which is available in your workspace. There is another outlier in the `annual_inc` variable with a max salary of $6 Mn. Coincidentally, we can see that it is the same person with 144 years of `age` who also earn an abnormally high salary of $6 Mn. Giving us more impetus to eliminate this observation from our dataset.

Lets start getting our hands dirty and do some data cleaning!

*** =instructions
- The first set of code is used to get the row number of the person with `age` = 144
- The second set of code is used to get the row number of the millionaire. Run this code to see that both these values correspond to the same row or observation.
- Remove this observation and store the cleaned data in `loan_data_1`.
- Check out the `str()` of `loan_data_1`.

*** =hint
- Use `which()` to determine the row index.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

loan_data <- read.csv("http://assets.datacamp.com/production/1608/loan_data.csv", stringsAsFactors = FALSE)

```

*** =sample_code
```{r}
# loan_data is available in your workspace
# Obtain the row number of the person with age 144
outlier_1 <- loan_data$age > 100
which(outlier_1)

# Obtain the row number of the person with annual_inc of 6000000
outlier_2 <- loan_data$annual_inc == 6000000
which(outlier_2)

# Remove the outliers and save the data: loan_data_1
loan_data_1 <- loan_data[-___(___), ]

# Look at the structure of loan_data_1

```

*** =solution
```{r}
# loan_data is available in your workspace
# Obtain the row number of the person with age 144
outlier_1 <- loan_data$age > 100
which(outlier_1)

# Obtain the row number of the person with annual_inc of 6000000
outlier_2 <- loan_data$annual_inc == 6000000
which(outlier_2)

# Remove the outliers and save the data: loan_data_1
loan_data_1 <- loan_data[-which(outlier_1), ]

# Look at the structure of loan_data_1
str(loan_data_1)

```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_output_contains("str(loan_data_1)",
                         incorrect_msg = "Have you correctly removed the outlier using [-which(outlier_1), ]?

test_error()

success_msg("Nice Job!")
```
