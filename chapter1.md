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

test_function("head", args = "n",
              not_called_msg = "You didn't call `head()`!",
              incorrect_msg = "You didn't call `head(.., n = ...)` with the correct argument, `n`.")
              
test_function("tail", args = "n",
              not_called_msg = "You didn't call `tail()`!",
              incorrect_msg = "You didn't call `tail(.., n = ...)` with the correct argument, `n`.")
              
test_function("str", args = "object",
              not_called_msg = "You didn't call `str()`!",
              incorrect_msg = "You didn't call `str(object = ...)` with the correct argument, `object`.")
              
test_function("hist", args = "x",
              not_called_msg = "You didn't call `hist()`!",
              incorrect_msg = "You didn't call `hist(x =)` with the correct argument, `x`.")
              
test_function("hist", args = "main",
              incorrect_msg = "You didn't call `hist(.., main =)` with the correct argument, `main`.")
              
test_function("hist", args = "xlab",
              incorrect_msg = "You didn't call `hist(.., xlab =)` with the correct argument, `xlab`.")
              
test_function("hist", args = "ylab",
              incorrect_msg = "You didn't call `hist(.., ylab =)` with the correct argument, `ylab`.")

# test_error()

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
- Check out the `summary` of `loan_data`.
- The first set of code is used to get the row number of the person with `age` = 144
- The second set of code is used to get the row number of the millionaire. Run this code to see that both these values correspond to the same row or observation.
- Remove this observation and store the cleaned data in `loan_data_1`.
- Now look at the `summary` of `loan_data_1`.

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

# Look at the summary of loan_data


# Obtain the row number of the person with age 144
outlier_1 <- loan_data$age > 100
which(outlier_1)

# Obtain the row number of the person with annual_inc of 6000000
outlier_2 <- loan_data$annual_inc == 6000000
which(outlier_2)

# Remove the outliers and save the data: loan_data_1
loan_data_1 <- loan_data[-___(___), ]

# Look at the summary of loan_data_1

```

*** =solution
```{r}
# loan_data is available in your workspace

# Look at the summary of loan_data
summary(loan_data)

# Obtain the row number of the person with age 144
outlier_1 <- loan_data$age > 100
which(outlier_1)

# Obtain the row number of the person with annual_inc of 6000000
outlier_2 <- loan_data$annual_inc == 6000000
which(outlier_2)

# Remove the outliers and save the data: loan_data_1
loan_data_1 <- loan_data[-which(outlier_1), ]

# Look at the summary of loan_data_1
summary(loan_data_1)

```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_output_contains("summary(loan_data_1)",
                         incorrect_msg = "Have you correctly removed the outlier using [-which(outlier_1), ]?")

test_error()

success_msg("Nice Job! Did you notice from the summary that we have couple more issues with data that need to be fixed? If not, again look carefully at the summary of loan_data_1")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:70d50d4c43
## Data Cleaning (2)

Data scientist spend around 3/4th of their time cleaning and tidying the data and spend the rest on analysis. This is an essential skill if you want to be a top Data scientist.

If you observed the `summary` of the `loan_data_1` you would have noticed that there are two variables in our dataset that have NAs. These are `int_rate` and `emp_length`. We can get the percentage of NAs in our dataset using:

`mean(is.na(column_name)) * 100`

Depending on the percentage of NAs and other factors we can arrive at a decision on how to handle NAs. The percentage of NAs in `int_rate` is 9.54% which is considerably high. In this case, we can replace all the NAs in `int_rate` column with the median `int_rate`

Lets do some more cleaning!

*** =instructions
- Look at the percentage NAs in `int_rate` column.
- Calculate the median `int_rate` and save it in `med_ir`
- Replace all NAs in `int_rate` column with `med_ir` and save it in a new data frame `loan_data_2`
- Now look at the `summary` of `loan_data_2`.

*** =hint
- Use `median(.., na.rm = TRUE)` to determine the median `int_rate`.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

loan_data_1
```

*** =sample_code
```{r}
# loan_data_1 is loaded in your workspace

# What is the percentage of NAs in int_rate?


# Row index of NAs in int_rate column
na_index <- which(is.na(loan_data_1$int_rate))

# Calculate the median int_rate
med_ir <- 

# Make a copy of loan_data_1
loan_data_2 <- loan_data_1

# Replace all NAs in int_rate column with med_ir: loan_data_2
loan_data_2$int_rate[___] <- ____

# Summary of loan_data_2

```

*** =solution
```{r}
# loan_data_1 is loaded in your workspace

# What is the percentage of NAs in int_rate?
mean(is.na(loan_data_1) * 100

# Row index of NAs in int_rate column
na_index <- which(is.na(loan_data_1$int_rate))

# Calculate the median int_rate
med_ir <- median(loan_data_1$int_rate, na.rm = TRUE)

# Make a copy of loan_data_1
loan_data_2 <- loan_data_1

# Replace all NAs in int_rate column with med_ir: loan_data_2
loan_data_2$int_rate[na_index] <- med_ir

# Summary of loan_data_2
summary(loan_data_2)
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_output_contains("summary(loan_data_2)",
                         incorrect_msg = "Have you correctly replaced the NAs using [na_index]?")

test_error()

success_msg("Good Job! Next lets work on the NAs in the other column!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ad39e12c95
## Data Cleaning (3)

If the percentage of NAs in a column are very large then it is advisable to investigate further if NAs have a special meaning else we can simply eliminate the column.

On the other hand if the percentage of NAs in a column are significantly low, we can look at means like previous exercise to replace the NAs if possible or eliminate entire rows in extreme cases.

In the `emp_length` column the percentage of NAs is 2.78% of total observations. Lets go ahead with the decision to eliminate these from our dataframe

*** =instructions
- Look at the percentage NAs in `emp_length` column.
- Remove all observations having NAs in `emp_length` column and save the clean dataset to `loan_data_3`.
- Look at the `summary` of `loan_data_3`. 
- Do you notice any more anomolies? If not, then lets finalize our data and save it for further analysis as `clean_data`.

*** =hint
- Use `loan_data_2` in `na.omit()` to eliminate all observations with NAs in the dataset.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

loan_data_1
loan_data_2
```

*** =sample_code
```{r}
# loan_data_1 and loan_data_2 are loaded in your workspace

# What is the percentage of NAs in emp_length of loan_data_1?


# Remove all NAs from the dataset loan_data_2 and save it in loan_data_3
loan_data_3 <- na.omit(____)

# Summary of loan_data_3


# Save loan_data_3 to clean_data for further analysis
clean_data <- ___
````

*** =solution
```{r}
# loan_data_1 and loan_data_2 are loaded in your workspace

# What is the percentage of NAs in emp_length of loan_data_1?
mean(is.na(loan_data_1$emp_length)) * 100

# Remove all NAs from the dataset loan_data_2 and save it in loan_data_3
loan_data_3 <- na.omit(loan_data_2)

# Summary of loan_data_3
summary(loan_data_3)

# Save loan_data_3 to clean_data for further analysis
clean_data <- loan_data_3
````

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_output_contains("summary(loan_data_3)",
                         incorrect_msg = "Have you correctly removed all NAs using [na_index]?")

test_error()

success_msg("Awesome!")
```

