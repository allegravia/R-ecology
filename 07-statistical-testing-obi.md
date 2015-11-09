---
layout: topic
title: Statistical testing
author: Data Carpentry contributors
minutes: 30
---
> ## Learning Objectives
>
> *   perform basic statistical tests


# What is statistical testing?

A statistical hypothesis is an assumption about a population parameter. This assumption may or may not be true. Hypothesis testing refers to the formal procedures used by statisticians to accept or reject statistical hypotheses.

## Student's t-test

You can find a broad description of the [Student's t-test on the wiki](https://en.wikipedia.org/wiki/Student's_t-test). Here we want to use the t-test in case in which we compare a distribution against a mean. Let's assume we have determined the daily energy intake in kJ for 11 women and we have the data in a vector:

```
> daily.intake <- c(5260,5470,5640,6180,6390,6515,6805,7515,7515,8230,8770)

```
We can explore the data using simple descriptive statistics:

```
> summary(daily.intake)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   5260    5910    6515    6754    7515    8770
>
> mean(daily.intake)
  [1] 6753.636
> sd(daily.intake)
   [1] 1142.123
> quantile(daily.intake)
   0% 25% 50% 75% 100%
   5260 5910 6515 7515 8770
```

We know that the recommended value is want 7725 kJ/day, and we want to ask if this group of woman is systematically deviating from the recommended value.  

Assuming that data come from a normal distribution, the object is to test whether this
distribution might have mean μ = 7725. This is done with t.test:

```
> t.test(daily.intake,mu=7725)

	One Sample t-test

data:  daily.intake
t = -2.8208, df = 10, p-value = 0.01814
alternative hypothesis: true mean is not equal to 7725
95 percent confidence interval:
 5986.348 7520.925
sample estimates:
mean of x
 6753.636

```
## Two-sample t test

The two-sample t test is used to test the hypothesis that two samples may
be assumed to come from distributions with the same mean


```
energy

```
