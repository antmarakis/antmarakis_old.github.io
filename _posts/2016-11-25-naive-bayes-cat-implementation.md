---
layout: post
title: Naive Bayes Categorical Classifier - Implementation
category: Artificial Intelligence
---


In this tutorial we will implement the <i>Naive Categorical Bayes Classifier</i>. [Previously](/naive-bayes-cat-read-data/) we looked at how we can read data from a text file. Here we will use this data to classify items.

### Approach

We want to determine which class is more likely to have the given features. For that, we will use the Bayes Equation:

$$P(Class|Features) = P(Class) * P(Features|Class) / P(Features)$$

We will also assume that the features are independent (hence the <i>Naive</i> in the name) so the above is simplified to:

$$P(Class|Features) = P(Class) * P(Feature_1|Class) * ... * P(Feature_n|Class) / (P(Feature_1) * ... * P(Feature_n))$$

The higher this number the better. So, we will run this equation for every class, inputing the given features and we will pick the highest.

### Data

We will use the code we wrote [here](/2016/11/23/naive-bayes-cat-read-data/) to read the data. Put said code into a Python script named <i>_DataReader</i> and import it into your classifier script.

<pre>
import _DataReader as DataReader;
</pre>

Then, call the <i>Read</i> function of <i>DataReader</i> and get the dictionaries. Assume your data set file is named <i>data.txt</i>. In code:

<pre>
#Read data from file
data = DataReader.Read('data.txt');
Classes = data[0];
Features = data[1];
P = data[2];
</pre>

The data set we will use can be found <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/Naive%20Bayes/Categorical/data.txt">here</a>. We have two classes, <i>Detective</i> and <i>Brute</i>, and three features, <i>Tall</i>, <i>Slim</i> and <i>Smart</i>. Each line represents an item. The first word of a line is the class the item is classified in. <i>True</i> means that the item has the feature on that column, while <i>False</i> means it doesn't.

Say we have a new item that we know has the following features, <i>Tall</i> and <i>Slim</i>. We want to classify it into one of the two classes.

We will run it through the classifier function. We will name that function <i>Classifier</i> and it will take as input a list of features (called <i>Evidence</i>). To call it we write:

<pre>
Classifier(['Tall','Slim']); #The item features
</pre>

<i>Note: The data set has another feature, Smart, but we didn't use it here. It doesn't mean that the item doesn't have the feature, we just don't know.</i>

### Code

The main function that will categorize/classify items is called <i>Classifier</i> and it has a parameter (called <i>Evidence</i>) to receive a list of features.

First we want to parse the list into a string (called <i>evidence</i>), with features separated by a comma. For example, if we receive the list ['Tall', 'Slim'] we want to parse it as "Tall, Slim". That way we can save the classification for future use in an intuitive way. In Statistics we write P(Detective\|Tall, Slim). Using a dictionary, we will write it as P["Detective\|Tall, Slim"], where "Tall, Slim" will come from the variable <i>evidence</i> and "Detective" from the iteration of the classes.

We also want to check if the given features are actually part of the feature list, <i>Features</i>. We will check that and we will build the string at the same time. <i>Evidence</i> is the input list of features.

<pre>
#The string of evidence, so that we can save it in P.
evidence = '';

#Check if all evidence is also in Features
for e in Evidence:
    if e not in Features:
        #A given evidence does not belong in Features. Abort.
        print "Evidence list is erroneous"
        return;

    #Build the evidence string
    evidence += e + ', ';

#remove the last two chars, as they are ', '
evidence = evidence[:-2];
</pre>

Now we are ready to delve into the core of the algorithm. The Bayes Equation. We will run the equation for every class <i>c</i> in <i>Classes</i>, saving the value in P(c\|evidence):

$$P(c|evidence) = P(c) * P(evidence_1|c) * P(evidence_2|c) * ... * P(evidence_n|c) / (P(evidence_2) * P(evidence_2) * ... * P(evidence_n))$$

We will start with the <i>a priori</i> probability of the class, and we will multiply that with the conditional feature/class probability and divide it with the probability of the feature.

Then we will check if the current class has a higher probability of appearing than the current max, and if yes we will save it. To implement this "Find Max" procedure, we will use the temporary variables <i>m</i> (to hold the max) and <i>classification</i> (to hold the classification).

In code:

<script src="https://gist.github.com/MrDupin/27ac7ab65d46c22d7aef831b2a084f79.js"></script>

Finally, we will return the classification and the max:

<pre>
#With the evidence, the item belongs to classification
#with a prob of m
print classification, m;
</pre>

If you run the above code, you should receive this output:

<pre>
Detective 0.47619047619
</pre>

### Conclusion

And with that we have developed a <i>Naive Categorical Bayes Classifier</i> from the ground up. You can find the full code on my <a href="https://github.com/MrDupin/Machine-Learning/tree/master/Classifiers/Naive%20Bayes/Categorical">Github</a>.

Links to the rest of the series:

[Part 1](/naive-bayes-cat-intro/)

[Part 2](/naive-bayes-cat-read-data/)
