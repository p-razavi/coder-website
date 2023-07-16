---
author: Pooya Razavi
date: "2023-07-04"
description: Repeated-Measures t-test in R and Python
tags:
- t-test, R, Python
title: Conducting Repeated-Measures t-test in R and Python
---

# Repeated Measures t-test Tutorial: Analyzing Data using R and Python

## Repeated Measures t-test
The repeated measures t-test (also known as the paired-samples t-test) is used to determine whether there are significant differences between the means of two related groups in a within-subjects design. 

## Example Scenario
Let's consider an example from marketing. Suppose a company wants to assess the effectiveness of their new add campaign in changing their costumers' interest in a new product. They collect data from 250 participants who report their attitude towards the product prior to viewing the ad campaign (i.e., _prior_) and after viewing the ads (i.e., _post_). By conducting a repeated measures t-test, the company can determine if there is a statistically significant difference in user attitudes as a result of the ad campaign.

![consumer looking at ads](/images/consumer_looking_at_ads.png)

## Conducting Repeated Measures t-test in R

### Step 1: Simulating Data
First, we need to simulate the data for our example scenario. Let's assume we have a data frame called `data` containing columns for participant ID, pre-campaign attitudes (i.e., _prior_), and post-campaign attitudes (i.e., _post_).

```R
set.seed(97)  # For reproducibility

# Simulating data for the example scenario
data <- data.frame(
  participant_id = rep(1:250),
  prior = rnorm(250, mean = 5, sd = 1),
  post = rnorm(250, mean = 6, sd = 1)
)
```

### Step 2: Conducting Repeated Measures t-test
Next, we can perform the repeated measures t-test using the `t.test()` function in R. We need to specify the columns that includes the prior and post attitude data, and clarify that we want paired-samples t-test using `paired`.

```R
# Performing repeated measures t-test
t_test_result <- t.test(data$prior, data$post, paired = TRUE)

# Calculate effect size
effsize::cohen.d(data$prior, data$post, paired = TRUE)

# Print the t-test result
print(t_test_result)
```

The output provides the t-statistic (_t_ = -9.68), degrees of freedom (_df_ = 249), p-value (_p_ < .001), and 95% confidence interval [-1.07, -0.70]. The results indicate that the campaign had a statistically significant and large effect (_d_ = 0.89) on users' attitudes.


## Conducting Repeated Measures t-test in Python

### Step 1: Simulating Data
Similarly, we need to simulate data for our example scenario using Python. We can use the `NumPy` and `Pandas` libraries to create a data frame.

```python
import numpy as np
import pandas as pd

np.random.seed(97)  # For reproducibility

# Simulating data for the example scenario
data = pd.DataFrame({
    'participant_id': np.repeat(np.arange(1, 251), 1),
    'prior': np.random.normal(loc = 5, scale = 1, size = 250),
    'post': np.random.normal(loc = 6, scale = 1, size = 250)
})
```

### Step 2: Conducting Repeated Measures t-test
To perform the repeated measures t-test in Python, we can use the `scipy.stats` module's `ttest_rel()` function. Note that to calculate Cohen's _d_ for the repeated measures t-test, we can use the mean difference and the pooled standard deviation.

```python
from scipy import stats

# Performing repeated measures t-test
results = stats.ttest_rel(data['prior'], data['post'])

# Calculating 95% confidence interval
conf_interval = stats.t.interval(0.95, results.df, loc=np.mean(data['prior'] - data['post']), scale=stats.sem(data['prior'] - data['post']))

# Calculate Cohen's d
mean_diff = np.mean(data['post'] - data['prior'])
pooled_sd = np.std(data['post'] - data['prior'])
cohens_d = mean_diff / pooled_sd

# Print the t-test result
print("T-statistic:", results.statistic)
print("df:", results.df)
print("P-value:", results.pvalue)
print("Confidence Interval:", conf_interval)
print("Cohen's d:", cohens_d)
```

The output will display the calculated t-statistic (_t_ = -11.54) and p-value (_p_ < .001), and 95% confidence interval [-1.16, -0.82], which indicate the campaign had a statistically significant and large effect (_d_ = 0.73) on users' attitudes toward the new product. 


Congratulations! You have successfully conducted a repeated measures t-test using both R and Python.