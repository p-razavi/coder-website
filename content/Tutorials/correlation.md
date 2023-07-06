---
author: Pooya Razavi
date: "2023-07-05"
description: Correlation in R and Python
tags:
- correlation, R, Python
title: Conducting Correlation Analysis in R and Python
---

## Correlation Analysis
Correlation analysis is a statistical technique used to measure the strength and direction of the relationship between two variables. It helps determine whether there is an association between the variables and the extent to which they vary together. 

## Example Scenario
Let's consider a practical example where a teacher wants to analyze the relationship between the number of hours studied and the corresponding test scores of 350 students in her class. By conducting a correlation analysis, we can assess the strength and direction of the relationship between these variables.

## Conducting Correlation Analysis in R

### Step 1: Simulating Data
To illustrate the correlation analysis, let's simulate the data in R such that the correlation between the two variables is approximately 0.6. We will use the `mvtnorm()` package.

````{r}
library(mvtnorm)

set.seed(100)  # For reproducibility

# Set up the data parameters, create a covariance matrix, and simulate data
correlation <- 0.6
mean_values <- c(25, 75)

cov_matrix <- matrix(c(5^2, correlation * 5 * 10, correlation * 5 * 10, 10^2), nrow = 2)

simulated_data <- rmvnorm(n = 350, mean = mean_values, sigma = cov_matrix)
hours_studied <- simulated_data[, 1]
test_scores <- simulated_data[, 2]
````

### Step 2: Calculating the Correlation
Next, we can calculate the correlation coefficient using the `cor.test()` function in R. The output will provide the correlation coefficient, which ranges from -1 to 1. A positive value indicates a positive correlation, a negative value indicates a negative correlation, and values close to 0 indicate a weak or no correlation.

```{r}
# Conduct the correlation analysis
cor.test(hours_studied, test_scores)

```
In this case, we can see that these two variables have a relatively strong and positive correlation (_r_ = 0.58), and this correlation is statistically significant (_p_ < .001).


### Step 3: Visualizing the Correlation using ggplot2
To visualize the correlation, we can create a scatter plot using the `ggplot2` package in R. 
The scatter plot shows the relationship between the variables, and the trend line provides an indication of the direction and strength of the correlation.

```{r}
library(ggplot2)

# Create a scatter plot
ggplot(data = data.frame(hours_studied, test_scores), aes(x = hours_studied, y = test_scores)) +
  geom_point(color = "purple") +
  geom_smooth(method = "lm", se = FALSE) +
  labs(title = "Correlation Analysis", x = "Hours Studied", y = "Test Scores") +
  geom_text(x = 15, y = 85, label = paste("r =", round(correlation, 2))) +
  theme_bw()
```


## Conducting Correlation Analysis in Python

We can follow the same steps in Python. We first simulate the data using the `np.random.multivariate_normal()` function to generate two variables (`hours_studied` and `test_scores`) with the specified means and covariance matrix. Then, we calculate the correlation coefficient using the `pearsonr()` function from `scipy.stats`. Finally, we create a scatter plot using `sns.regplot()` from the _seaborn_ library and add the correlation coefficient as text using `plt.text()`.

### Step 1: Simulating Data

```{python}
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy.stats import pearsonr

np.random.seed(100)  # For reproducibility

# Set up the data parameters, create a covariance matrix, and simulate data
correlation = 0.6
mean_values = [25, 75]
cov_matrix = [[5 ** 2, correlation * 5 * 10], [correlation * 5 * 10, 10 ** 2]]

simulated_data = np.random.multivariate_normal(mean=mean_values, cov=cov_matrix, size=350)
data = pd.DataFrame(simulated_data, columns=['hours_studied', 'test_scores'])
```

# Step 2: Calculating the correlation coefficient

```{python}
# Calculating the correlation coefficient, p-value, and degrees of freedom
correlation, p_value = pearsonr(data['hours_studied'], data['test_scores'])
df = len(data) - 2  # Degrees of freedom

print("Correlation coefficient:", correlation)
print("P-value:", p_value)
print("Degrees of Freedom:", df)
```

# Create a scatter plot

```{python}
sns.regplot(data=data, x='hours_studied', y='test_scores')
plt.title("Correlation Analysis")
plt.xlabel("Hours Studied")
plt.ylabel("Test Scores")
plt.text(10, 90, f"Correlation: {correlation:.2f}")
plt.show()
```

Congratulations! You have successfully conducted correlation analysis using both R and Python.