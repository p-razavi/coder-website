---
author: Pooya Razavi
date: "2023-07-04"
description: Independent Samples t-test in R and Python
tags:
- t-test, R, Python
title: Conducting Independent Samples t-test in R and Python
---

## Independent Samples t-test
The independent samples t-test (or between-subjects t-test) is used to determine whether there are statistically significant differences between the means of two independent groups. 

## Example Scenario
Let's consider a simple example from psychology. Suppose a researcher wants to investigate whether there is a significant difference in the mean anxiety levels between two groups of participants: Group A, which received a mindfulness intervention, and Group B, which did not receive any intervention. The researcher believes that mindfulness can reduce anxiety levels. Assuming that the two groups, on average, had the same level of anxiety prior to the intervention, by conducting an independent samples t-test, the researcher can compare the post-intervention anxiety levels and assess whether the intervention had a significant impact on anxiety.

## 3. Conducting Independent Samples t-test in R

### Step 1: Simulating Data
First, we need to simulate the data for our example scenario. Let's assume that each group consists of 90 participants, and anxiety levels are measured on a scale from 1 to 10. We will use the `rnorm()` function to generate normally distributed random data.

```R
set.seed(110)  # For reproducibility

# Simulating data for Group A
group_a <- rnorm(90, mean = 6, sd = 1)

# Simulating data for Group B
group_b <- rnorm(90, mean = 7, sd = 1)
```

### Step 2: Conducting Independent Samples t-test
Next, we can perform the independent samples t-test using the `t.test()` function in R.

```R
# Performing independent samples t-test
t_test_result <- t.test(group_a, group_b)

# Calculate effect size using the effsize package
effsize::cohen.d(group_a, group_b)

# Print the t-test result
print(t_test_result)
```

The output provides the t-statistic (_t_ = -6.59), degrees of freedom (_df_ = 177.9), p-value (_p_ < .001), and 95% confidence interval (-1.19, -0.64). These results indicate that the difference between the two groups is significant, and effect size is large (_d_ = 0.98).

## 4. Conducting Independent Samples t-test in Python

### Step 1: Simulating Data
Similarly, we need to simulate data for our example scenario using Python. We can use the NumPy library to generate random data from normal distributions.

```python
import numpy as np

np.random.seed(110)  # For reproducibility

# Simulating data for Group A
group_a = np.random.normal(loc=6, scale=1, size=90)

# Simulating data for Group B
group_b = np.random.normal(loc=7, scale=1, size=90)
```

### Step 2: Conducting Independent Samples t-test
To perform the independent samples t-test in Python, we can use the `scipy.stats` module's `ttest_ind()` function.

```python
from scipy import stats

# Performing independent samples t-test
t_statistic, p_value = stats.ttest_ind(group_a, group_b)

# Print the t-test result
print("T-statistic:", t_statistic)
print("P-value:", p_value)
```

The output will display the calculated t-statistic  (_t_ = -6.89) and p-value (_p_ < .001), which indicates that the difference between the two groups is statistically signification. Note that the calculation of effect size in Python needs some manual calculation. The results from the code below indicates that the difference between the two group is large (_d_ = 1.03):

```python
import numpy as np
from scipy import stats

# Calculate Cohen's d
mean_diff = np.mean(group_a) - np.mean(group_b)
pooled_sd = np.sqrt(((len(group_a) - 1) * np.var(group_a) + (len(group_b) - 1) * np.var(group_b)) / (len(group_a) + len(group_b) - 2))
cohens_d = mean_diff / pooled_sd

# Print Cohen's d
print("Cohen's d:", cohens_d)
```


Congratulations! You have successfully conducted an independent samples t-test using both R and Python.