---
layout: post
title: Naive Bayes Numerical Classifier - Implementation
category: Artificial Intelligence
---

We have talked a bit about how to read and process a text file into information usable by the classifier. Now we will take a look at the implementation of the Numerical Naive Bayes algorithm. An example of the text file holding the data is the following:

<pre>
Class Height Weight
Wrestler 170 61
Wrestler 173 67
Sumo 181 110
Sumo 177 100
Wrestler 175 69
Sumo 180 111
</pre>

For this tutorial, we will use the data set found <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/Naive%20Bayes/Numerical/data.txt">here</a>.

### Distribution Function

We will also assume the data set follows the Gaussian Distribution. You can read on the gaussian distribution on <a href="https://en.wikipedia.org/wiki/Normal_distribution">Wikipedia</a>. What you need to know is that it takes as input an item <i>x</i> and given the standard deviation and mean of the distribution returns the probability of <i>x</i> occuring. So, we only need to compute <i>σ</i> (standard deviation) and <i>µ</i> (mean) and then we can find the probability of any value.

To find the conditional probability P(evidence\|class), we want to get the output of the above formula. Therefore, we will write a function for it, named <i>Gaussian</i>. This will take as input the mean, the standard deviation and the value x.

<pre>
def Gaussian(mean,stDev,x):
    pi = math.pi;
    e = math.e;
    m = mean;
    s = stDevl

    g = 1/(math.sqrt(2*pi)*s)*e**(-0.5*(float(x-m)/s)**2);

    return g;
</pre>

Notice we have calls to <i>math.sqrt</i>, <i>math.pi</i> etc. so we will need to import the library <i>math</i>.

<pre>
import math;
</pre>

### Code

#### Reading from File

First we will need to build the dictionaries and lists <i>Classes</i>, <i>Features</i>, <i>P</i> and get the count of the items in our data set, <i>n</i>.

We have already build a [data reader](/naive-bayes-num-read-data/). We will save that code into a Python script called <i>_DataReader</i> and we will import it here.

<pre>
import _DataReader as DataReader;
</pre> 

We will call its <i>Read</i> function and we will pass as parameter the file name of the data set. This will return a list, which we will parse into the appropriate variables:

<pre>
Classes, Features, P, n = DataReader.Read('data.txt');
</pre>

#### Classifier

Let our classifier function be called <i>Classifier</i>. It takes as parameter a tuple, <i>Evidence</i> which holds tuples of two items, the name of the feature and the value of the feature. This is how we call the function:

<pre>
#Run classifier with the evidence list
Classifier((('Height', 170), ('Weight', 65)));
</pre>

That means the item we want to classify has Height of 170 and Weight of 65.

First we want to parse <i>Evidence</i> into a string (called <i>evidence</i>), with features separated by a comma. For example, if we receive (('Height', 170), ('Weight', 65)) we want to parse it to "Height = 170, Weight = 65". That way we can save the classification for future use in an intuitive way. In Statistics we write P(Wrestler\|Height = 170, Weight = 65). Using a dictionary, we will write it as P["Wrestler\|Height = 170, Weight = 65"], where "Height = 170, Weight = 65" will come from the variable <i>evidence</i> and "Wrestler" from the iteration of the classes.

We also want to check if the given features are actually part of the feature list, <i>Features</i>. We will check that and we will build the string at the same time. <i>Evidence</i> has two-length lists; the first element the feature name and the second the feature value. We will save those two values on the variables <i>eF</i> and <i>eV</i> respectively. We will check if <i>eF</i> is in <i>Features</i>.

<pre>
#Check if all evidence is also in Features
    for e in Evidence:
        eF = e[0]; #The feature in evidence e
        eV = e[1]; #The value in evidence e
        if eF not in Features:
            #A given evidence does not belong in Features. Abort.
            print "Evidence list is erroneous"
            return;

        #Build the evidence string
        evidence += eF + " = " + str(eV) + ', ';

    evidence = evidence[:-2]; #remove last two chars
</pre>

Now we reach the core of the algorithm. We will run the Bayes equation for all classes and we will pick the largest value.

What we are essentially doing is finding the value P(c\|evidence) for each <i>c</i> in <i>Classes</i>. The Bayes equation is P(c\|evidence) = P(evidence\|c)\* P(c)/P(evidence). Because though the features are independent (hence the <i>Naive</i> algorithm) we can simplify the above into \\( P(c|evidence) = P(evidence_1|c) * P(evidence_2|c) * ... * P(evidence_n|c) * P(c) \\). We do not need to calculate P(evidence), since it is the same for all classes and does not play a role in the algorithm (also, it's hard to compute). We know what P(c) is, but we are missing the probability of a feature given a class, \\( P(evidence_i|c) \\). To calculate it, we will use the Gaussian Formula from before.

To the Gaussian function we have created, we will input the mean and the standard deviation of the feature given the class. (class name is <i>c</i>, feature name is <i>eF</i>)

<pre>
Classes[c][eF]["Mean"]
Classes[c][eF]["StDev"]
</pre>

We will also input the feature value, <i>eV</i>. We will do this for every class and every feature in <i>Evidence</i>.

Lastly, we need to keep the class with the max value. A simple "Find Max" problem. To implement this we will use the auxiliary variables <i>m</i> (for the max) and <i>classification</i> (for the class).

In code:

<pre>
for c in Classes:
        #Start from the prior probability
        P[c + '|' + evidence] = P[c];
        
        for e in Evidence:
            eF = e[0]; #The feature in evidence e
            eV = e[1]; #The value in evidence e
            #Multiply by the conditional prob
            mean = Classes[c][eF]["Mean"]; #mean
            stDev = Classes[c][eF]["StDev"]; #standard deviation
            P[c + '|' + evidence] *= Gaussian(mean,stDev,eV);

        if(P[c + '|' + evidence] > m):
            #P(c|evidence) is the max so far;
            #update m and classification
            m = P[c + '|' + evidence];
            classification = c;
</pre>

To print the solution/classification of the item, we write:

<pre>
#With the evidence, the item belongs to classification
#with a prob of m
print classification, m;
</pre>

If you run the above code, you should receive as output:

<pre>
Wrestler 0.00346294406914
</pre>

<h3>Conclusion</h3>

And with that we have developed a <i>Naive Numerical Bayes Classifier</i> from the ground up. You can find the full code on my <a href="https://github.com/MrDupin/Machine-Learning/tree/master/Classifiers/Naive%20Bayes/Numerical">Github</a>.

<hr>

[Part 1](/naive-bayes-num-intro/)

[Part 2](/naive-bayes-num-read-data/)
