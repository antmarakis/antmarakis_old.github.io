---
layout: post
title: Naive Bayes Numerical Classifier - Reading Data
category: Artificial Intelligence
---

This process is very similar to the [Categorical Classifier one](/naive-bayes-cat-read-data/). We will again need to calculate the dictionaries <i>Classes</i>, <i>Features</i> and <i>P</i> (for probabilities). There are some key differences though. Since the values now are numerical, you will be using a distribution function instead of calculating individual probabilities. That means we do not need individual feature probabilities.

We assume the data set follows the Gaussian distribution.

### Example File

For simplicity reasons, we will be using a text file to read our data. An example test file is the following:
<pre>Class Height Weight
Wrestler 170 61
Wrestler 173 67
Sumo 181 110
Sumo 177 100
Wrestler 175 69
Sumo 180 111
</pre>

### Features
The features will be saved in a list, <i>Features</i>. We just need to extract their names from the first line of the text file, and we are done.

### Probabilities

The only probabilities we will need are the <i>a priori</i> probabilities of the classes. We will calculate those and put them in a dictionary. Their value is the total of items in the class divided by the total of items in general.

### Classes
To save the classes information, we will create a dictionary named <i>Classes</i>.

Inside this dictionary, we will new another dictionary for all the features in our data set. The keys to this nested dictionary are the names of the features. Inside <i>those</i> dictionaries we will need another dictionary for the standard deviation and the mean. The keys are respectively <i>StDev</i> and <i>Mean</i>. They hold the standard deviation and mean for the feature in the class. That way when we want to calculate P(feature<sub>i</sub>\|class) we only need to look at the <i>StDev</i> and <i>Mean</i> in the inner dictionary and together with the value we are checking we input them to the Gaussian distribution equation.

We will also need to know the total of items in a class. We will save this value in a new element in the second dictionary, under the key "Total".

Visually the <i>Classes</i> dictionary is this:

<img src="http://i.imgur.com/17k2zQ4.png" alt="classes" width="540" height="195" />

Below are a few calls to the dictionary, to get a feel of how it is structured code-wise:
<pre>Classes["Wrestler"]["Total"]; #number of items in Wrestler
Classes["Sumo"]["Height"]["Mean"]; #mean of Height in Sumo
</pre>

### Mean and Standard Deviation

Mean is basically the average value. We will calculate it using the approach I explained [in this post](/calculating-averages/).

The Standard Deviation is the square root of the Variance, and Variance is the average of the squared differences from the Mean. There are two Standard Deviations though, Population and Sample. Population Standard Deviation is the average we know and use, dividing by the count of items. Sample is a little different; we divide by the count <i>minus 1</i>. Usually in Computing Statistics, we use the Sample instead of the Popularity one, and we will do the same here. It doesn't make much of a difference; we just use the more common one.

### Implementation

For <i>Classes</i> we need to calculate the Mean and Standard Deviation for every combination of class and feature. I will not go into much detail, as it's just technicalities and as an idea it is simple enough.

First you need to calculate the Mean, as it is needed to calculate the Deviation. With the Mean, you will need to also calculate the Total of classes. Once you've done that, you will move to the calculation of the Deviation. It is useful to save the the Variances in a temporary dictionary similar to the nested dictionary of <i>Classes</i> for features. You will build this dictionary and then for each of its values you will square it and save it in the nested <i>Classes</i> dictionary.

For the features, you simply need to save their names to a list. For the probabilities, you need to find the probability of a class appearing in general.

At the end you return the <i>Classes</i>, <i>Features</i>, <i>P</i> and <i>n</i> (the total count of items).

You can find the code with more commenting/description <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/Naive%20Bayes/Numerical/_DataReader.py">on my Github.</a>

<hr>

[Part 1](/naive-bayes-num-intro/)

[Part 3](/naive-bayes-num-implementation/)
