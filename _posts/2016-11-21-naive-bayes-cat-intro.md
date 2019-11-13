---
layout: post
title: Naive Bayes Categorical Classifier - Intro
category: Artificial Intelligence
---

This is the first part of a three-part tutorial that will serve as a guide to a very popular and robust Pattern Recognition algorithm, the Naive Bayes Classifier. First we will take a look at the categorical classifier, which means the input has discrete values ('Red', 'Tall', integers in a range, etc). Later we will also take a look at the numerical classifier, for continuous values (for example, real numbers representing height).

You can find the code we will go through in this tutorial on my <a href="https://github.com/MrDupin/Machine-Learning/tree/master/Classifiers/Naive%20Bayes/Categorical">GitHub</a>.

### Intro

We are given a data set containing information about the categorization of items into classes. We are also given a new item and we want to classify it. This is a common problem in data science and pattern recognition. We will solve it using the <i>Naive Categorical Bayes Classifier</i>.

Let's dive right in.

### Data Set

First we will talk a bit about the data set. For this example, we will assume the data set is a simple text file with the entries separated by a new line. Each entry represents an item, the information we know about it and the class it is categorized in. The information about the items is <i>categorical</i>, which basically means the values are discrete. These values may be "Red", "Blue", "Purple" etc. or they may be simple boolean values. For example, take this data set with boolean values:

<table>
  <tr>
    <th>Class</th>
    <th>Tall</th> 
    <th>Slim</th>
    <th>Smart</th>
  </tr>
  <tr>
    <td>Detective</td>
    <td>True</td>
    <td>True</td>
    <td>True</td>
  </tr>
  <tr>
    <td>Brute</td>
    <td>True</td>
    <td>False</td>
    <td>False</td>
  </tr>
</table>

Here we have two objects. One is "Tall", "Slim" and "Smart", while the other one is "Tall", but is neither "Slim" nor "Smart". The first one is classified under "Detective" while the other one under "Brute".

"Tall", "Slim" and "Smart" are called <i>features</i>. Under "Class", we see the class the item is classified in. In a way the first line asks questions that the lines below answer. Note that it is wise to also have the complement of features in boolean sets (eg. "Not Tall"), as it will make implementation easier. Complements are not hard to compute though so we can leave it out of the data set (the value of "Not Tall" is the opposite of "Tall", which we can easily calculate once we know "Tall").

Keep in mind that multiple items can be categorized under the same class, even if they do not have the same features.

### Pre-Processing

We have the data set, now we need to make some sense of it so we can properly use it. We will calculate all the probabilities we will need.

How probable is it that an item is "Tall"? "Slim"? A "Detective"? Given that an item is a "Detective", how probable is that it is also "Tall"? We want to answer all these questions.

<pre>
P(Tall) = ?
P(Slim) = ?
P(Detective) = ?
P(Tall|Detective) = ?
</pre>

Let the probabilities be the following:

Prior Probabilities of the Classes (or <i>a priori</i>)
<pre>
P(Detective) = 0.7
P(Brute) = 0.3
</pre>

Features Probabilities
<pre>
P(Tall) = 0.5
P(Slim) = 0.4
P(Smart) = 0.3
</pre>

And finally the <i>conditional</i> probabilities. I won't go in detail what a conditional probability is, but in a nutshell it means, given B happened, what is the chance that A will happen? We write P(A\|B).

Likelihood Probabilities (aka conditional probabilities)
<pre>
P(Tall|Detective) = 0.7
P(Smart|Brute) = 0.05
...
...
...
P(Not Tall|Detective) = 0.3
P(Not Smart|Brute) = 0.95
</pre>

### Bayes Equation

We have all these probabilities, but how do we use them to classify new items? The answer is given via the Bayes Equation, which gives the probability of A happening given B happened/is known.

<p align="center">P(A | B) = P(A) * P(B | A) / P(B)</p>

When we are given a new item, we know its features. We will use these features to decide in which class the item most likely belongs. We will denote class by "Class" and the features by "Features". The equation becomes:

<p align="center">P(Class|Features) = P(Class) * P(Features|Class) / P(Features)</p>

In this case though, Features is not a single variable. It is a vector of variables, \\( (Feature_1, Feature_2, ..., Feature_n) \\). More accurately, it is a conjunction of variables, \\( Feature_1 AND Feature_2 AND ... AND Feature_n \\). That is an issue, as calculation now becomes complicated. To solve this, we assume that all the features are independent and therefore the calculation becomes easier. From the <i>multiplication rule of probability</i>, we know that when the variables are independent, we have:

$$P(X_2) \cap P(X_2 \cap ... \cap P(Xn))$$

Which for conditional probabilities becomes:

$$P(X_1, X_2, ... , X_n | Y) = P(X_1 | Y) * P(X_2 | Y) * ... * P(X_n | Y)$$

So the equation can be further simplified to:

$$P(Class|Features) = P(Class) * P(Feature_1 | Class) * ... * P(Feature_n | Class) / P(Feature_1) * ... * P(Feature_n)$$

Now we know all the probabilities and the equation and can thus find the probability of a class given the item's features.

### Classification

Remember, we need to find in which class the new item should be classified under. With the Bayes Equation we can find the probability an item with a certain set of features belongs to a class. How can we use this to our advantage?

It's easy! We will run the equation for all the classes and pick the highest value. That class will be the solution to our problem.

Some little info about the result:

a) What we are calculating is the conditional probability of a class based on the set of evidence. P(Class\|Evidence)

b) The higher this value, the more certain we are that we picked the correct class.

c) The value is <i>not</i> the probability of the class based on the evidence. Even though a lot of people on the internet say it is, that is not the case. Because remember that at the beginning of the algorithm, we only <i>assumed</i> that the features are independent. That may not actually be the case. If the features are dependent, our formula does not work properly. It can produce a value greater than 1! This little error does not matter much. Only when we are dealing with very large data will we notice a difference.

d) The resulting class might not be correct. The algorithm relies on probabilities, so the result is also probabilistic and not absolute. If the data set is large enough though, it is highly probable the algorithm will not err.

e) The Bayesian Classifier does not need a huge data set to function properly. Even with little information, it can function at a very high level of accuracy.

f) We do not actually have to calculate the denominator in the formula, as it is the same for all classes. It's the probability of the features, which doesn't depend on the classes at all, so it will remain the same. We can do away with it without any issue.

### Example

Let's return to our example of Detectives and Brutes. We have been given our data set which we translated to the following table:

<table>
  <tr>
    <th>Class</th>
    <th>Tall</th> 
    <th>Slim</th>
    <th>Smart</th>
    <th>Total</th>
  </tr>
  <tr>
    <td>Detective</td>
    <td>10</td>
    <td>10</td>
    <td>15</td>
    <td>20</td>
  </tr>
  <tr>
    <td>Brute</td>
    <td>25</td>
    <td>5</td>
    <td>5</td>
    <td>30</td>
  </tr>
  <tr>
    <td>Total</td>
    <td>35</td>
    <td>15</td>
    <td>20</td>
    <td>50</td>
  </tr>
</table>

Evidence Probabilities:

<pre>
P(Tall) = 0.7 (35/50)
P(Slim) = 0.3 (15/50)
P(Smart) = 0.4 (20/50)
</pre>

Prior Probabilities:

<pre>
P(Detective) = 0.4 (20/50)
P(Brute) = 0.6 (30/50)
</pre>

Likelihood Probabilities:

<pre>
P(Tall|Detective) = 0.5
P(Slim|Detective) = 0.5
...
...
...
P(Smart|Brute) = 0.1666
P(Not Slim|Brute) = 0.8333
</pre>

We are now given a new item to classify. This item will have the following features:

Tall, Slim.

We input them to our formula and we run it for all the classes, to see which produces the highest result:

For "Detective":

<p align="center">P(Detective|Tall, Slim) = P(Tall|Detective) * P(Slim|Detective) * P(Detective) / (P(Tall)*P(Slim))</p>

<p align="center">0.5*0.5*0.4/0.7*0.3 = 0.4761</p>

For "Brute":

<p align="center">P(Brute|Tall, Slim) = P(Tall|Brute) * P(Slim|Brute) * P(Brute) / (P(Tall) * P(Slim))</p>
<p align="center">0.8333*0.1666*0.6/0.7*0.3 = 0.3966</p>

The value for Detective is higher than for Brute (0.4761 > 0.3966) so the answer is Detective.

### Ending Notes

The <i>Naive Bayes Classifier</i> is a very useful tool in Data Science. Despite its simplicity, it produces highly accurate results, often competing with more complex algorithms.

The algorithm explained here is categorical (for discrete values), but the classifier can work for continuous values too, although the algorithm is slightly different (it uses density functions instead of probabilities).

Finally, the algorithm is very useful not only for its robustness, but also for its versatility. We can use any combination of the features we want in the classifier, which allows us to make a prediction for any item/feature combination.

---

You can find the rest of the series here:

<a href="https://mrdupin.github.io/naive-bayes-cat-read-data/">Part 2</a>

<a href="https://mrdupin.github.io/naive-bayes-cat-implementation/">Part 3</a>
