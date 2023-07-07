---
author: Pooya Razavi
date: "2023-07-07"
description: One-Way ANOVA in R and Python
tags:
- one-way ANOVA, R, Python
title: Conducting One-Way ANOVA in R and Python
---

## One-Way ANOVA
One-Way Analysis of Variance (ANOVA) is a statistical test used to compare the means of three or more groups or levels of a categorical independent variable. It determines if there are statistically significant differences between the group means.

## Example Scenario
In this example, we have a dataset that includes information on rental prices across three states: Oregon (OR), California (CA), and Washington (WA). The dataset contains 15,000 rows, with 5,000 rows for each state. We want to use a one-way ANOVA to analyze these data and compare the average rents between the three states.

## Conducting One-Way ANOVA in R

### Step 1: Loading the Data
To begin, we need to load the dataset into R and inspect its structure. You can download the dataset from  [this link on GitHub](https://github.com/p-razavi/sim_datasets/blob/main/US_west_rent.rds) by opening the link and clicking on the download icon. Alternatively, you can run the code below in R. 

```{r}
# load the data
url <- "https://github.com/p-razavi/sim_datasets/raw/main/US_west_rent.rds"
rent_data <- readRDS(url(url))

# inspect the data
dplyr::glimpse(rent_data)
```

### Step 2: Performing One-Way ANOVA
Next, we can perform the one-way ANOVA using the `aov()` function in R. We'll specify the formula with the dependent variable `rent` and the independent variable `state`. Note that we need the `effectsize` package to calculate the effect size (η²).

```R
# Perform one-way ANOVA
anova_result <- aov(rent ~ state, data = rent_data)

# Print the ANOVA table
summary(anova_result)

# Calculate effect size
effectsize::eta_squared(anova_result)
```

The output will display the ANOVA table, including the sum of squares, degrees of freedom, mean squares, F-statistic, and p-value. The results indicate that the rent prices, on average, differ significantly across the three western states, _F_(2, 14997) = 351.3, _p_ < .001, η² = 0.04.

### Step 3: Creating a Figure using ggplot
To visualize the differences between the states' rents, we can create a box plot using the `ggplot2` package in R.

```R
library(ggplot2)

# Create a box plot
ggplot(rent_data, aes(x = state, y = rent, fill = state)) +
  geom_boxplot() +
  labs(title = "Comparison of Rents across States", x = "State", y = "Rent") +
  theme_bw()
```

The box plot visually presents the distribution of rents for each state and allows for easy comparison of the median and quartiles. We can see that the rents are higher in California, followed by Washington and Oregon.

## Conducting One-Way ANOVA in Python

### Step 1: Loading and Inspecting the Data
First, we need to download the dataset from  [this link on GitHub](https://github.com/p-razavi/sim_datasets/blob/main/US_west_rent.rds) by opening the link and clicking on the download icon. Next, we can load the dataset using the `pyreadr` library. 

```python
import pandas as pd
import pyreadr

# Load the dataset
readr_result = pyreadr.read_r('C:/WordkingDirectory/US_west_rent.rds') # Enter your working directory (where the data file is saved) here.

# extract the pandas data frame 
rent_data = readr_result[None]

```

### Step 2: Performing One-Way ANOVA

To perform the one-way ANOVA in Python, we can use the `statsmodels` library.

```python
import statsmodels.api as sm
from statsmodels.formula.api import ols

model = ols('rent ~ C(state)', data=rent_data).fit()
aov_table = sm.stats.anova_lm(model, typ=3)
aov_table

```

Calculating the effect size needs some manual coding:

```{python}
import numpy as np

# Define the rent data for each state
rent_or = rent_data['rent'][rent_data['state'] == 'OR']
rent_ca = rent_data['rent'][rent_data['state'] == 'CA']
rent_wa = rent_data['rent'][rent_data['state'] == 'WA']
# Calculate the effect size (eta squared)
ss_between = (np.mean(rent_or) - np.mean(rent_data['rent']))**2 * len(rent_or) + \
             (np.mean(rent_ca) - np.mean(rent_data['rent']))**2 * len(rent_ca) + \
             (np.mean(rent_wa) - np.mean(rent_data['rent']))**2 * len(rent_wa)
ss_total = np.sum((rent_data['rent'] - np.mean(rent_data['rent']))**2)
eta_squared = ss_between / ss_total
```

As you can see, the output matched what we saw from R, indicating that the rent prices, on average, differ significantly across the three western states, _F_(2, 14997) = 351.3, _p_ < .001, η² = 0.04. 

### Step 3: Creating a Figure for the Results
To visualize the differences between the states' rents, we can create a box plot using the `matplotlib` library in Python.

```python
import matplotlib.pyplot as plt

# Create a box plot
plt.figure()
plt.boxplot([rent_data['rent'][rent_data['state'] == 'OR'],
             rent_data['rent'][rent_data['state'] == 'CA'],
             rent_data['rent'][rent_data['state'] == 'WA']],
            labels=['OR', 'CA', 'WA'])
plt.title("Comparison of Rents across States")
plt.xlabel("State")
plt.ylabel("Rent")
plt.show()
```

The box plot visually presents the distribution of rents for each state, allowing for easy comparison of the median and quartiles. We can see that the rents are higher in California, followed by Washington and Oregon.

Congratulations! You have successfully conducted a one-way ANOVA using both R and Python.