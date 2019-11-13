---
layout: post
title: Naive Bayes Numerical Classifier - Intro
category: Artificial Intelligence
---

We have previously seen how the Bayesian Classifier works for discrete features. I suggest you skim over [that post](/naive-bayes-cat-intro/) first, so that you will better understand how this type of classifier works. We will now take a look at the case where features have numerical values.

### Overview

A great example of this is a feature of height. Values to this feature are real numbers (1.77, 1.91, 1.67, etc). We cannot create dictionary entries for every single one of these values, because 1) we run the risk of creating more entries than we bargained for, and 2) the resulting probabilities might not be representative of the actual probabilities. For example, we might have a lot of values close to 1.70 (1.699, 1.68, 1.71 etc.) but with only one being actual 1.70 and only another one close to 1.80. So P(1.80) = P(1.70) even though we are a lot likelier to get a number closer to 1.70. This, in a lot of cases, is a problem.

How do we work around that? We will model the data set into a distribution function (Gaussian, Poisson, etc). Let's assume the data set follows a normal (*gaussian*) distribution. You can read on the gaussian distribution on <a href="https://en.wikipedia.org/wiki/Normal_distribution">Wikipedia</a>. What you need to know is that it takes as input an item *x* and given the standard deviation and mean of the distribution returns the probability of *x* occuring. So, we only need to compute <i>σ</i> (standard deviation) and <i>µ</i> (mean) and then we can find the probability of any value.

Of course, every class has a separate function for each feature. If we have <i>n</i> classes and <i>m</i> features, we are going to need to compute <i>n*m</i> functions (that is, compute their mean and standard deviation).

(The mean is the average of the values, and the standard deviation is the square root of the variance, and variance is the average of the squared differences from the mean. For the average of the variance though we divide not by the number of values, but the number of values minus 1. In Computational Statistics this the norm so we will use it here too. It doesn't make much of a difference. This average is called <i>Sample</i>. The average we are used to is called <i>Population</i> average.)

Let's take a look at the Bayes Equation for a Class given a set of Features:

$$P(Class|Features) = P(Class) * P(Feature_1|Class) * ... * P(Feature_n|Class) / (P(Feature_1) * ... * P(Feature_n))$$

We can easily calculate P(Class). We do not have to calculate the denominator, as it will be the same for each class and therefore will cancel out (also, it is very hard to compute). The last thing that remains is the conditional probabilities of the features given the class. That's where the Gaussian Distribution Function comes in. We will find the function of the class for the feature, and we will input the feature value.

So, we calculate all the class probabilities and pick the highest value. Essentially the only difference from the categorical classifier is the use of a distribution function for the conditional probabilities of feature/class and the prior calculation of the mean and standard deviation.

PS: Instead of the normal distribution, you can use any other distribution if you know your data follows it. It's just that the normal distribution is more common, so it's safe to assume the data falls under that distribution.

### Example

An example data set in a txt file.

<table>
  <tr>
    <th>Class</th>
    <th>Height</th> 
    <th>Weight</th>
  </tr>
  <tr>
    <td>Wrestler</td>
    <td>170</td>
    <td>61</td>
  </tr>
  <tr>
    <td>Wrestler</td>
    <td>173</td>
    <td>67</td>
  </tr>
  <tr>
    <td>Sumo</td>
    <td>181</td>
    <td>110</td>
  </tr>
  <tr>
    <td>Wrestler</td>
    <td>177</td>
    <td>73</td>
  </tr>  <tr>
    <td>Sumo</td>
    <td>175</td>
    <td>113</td>
  </tr>
</table>

First we need to find the mean and standard deviation for the classes for each feature.

For the Wrestler class and the Height feature we have:

<p align="center">mean = 170+173+177/3 = 173.333$

<p align="center">stDev = sqrt{ ((170-173.333)^2 + (173-173.333)^2 + (177-173.333)^2)/2 } = 3.5</p>

The probability of Wrestler appearing (irregardless of features) is 3/5.

We do the rest for each Class and Feature.

Then, when we receive a new item, say the {"Height":175, "Weight":65}, to find how probable it is that the item is a Wrestler, we run the Bayes equation. We get the conditional probabilities feature/class by inputting the feature values of the item into the Gaussian equation, which we create by using the mean and stDev of the class for the inputting feature. As we know the probability of the class appearing, we have:

<pre>
P(Wrestler|Height=175, Weight=65) = P(Height=175|Wrestler)*P(Weight=65|Wrestler)*P(Wrestler)
</pre>

where P(Wrestler) is 3/5 and we get the value of P(Height=175|Wrestler) from the Gaussian function, given the mean and standard deviation of the "Wrestler" class and the "Height" feature, for *x = 175*. We do the same for the 'Weight=65' and we have a value for P(Wrestler|Height=175, Weight=65). We then work similarly for the Sumo class and we pick the largest value, which will be our predicted classification.

<hr>

<a href="https://mrdupin.github.io/naive-bayes-num-read-data/">Part 2</a><br><br><a href="https://mrdupin.github.io/naive-bayes-num-implementation/">Part 3</a>
