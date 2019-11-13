---
layout: post
title: Naive Bayes Categorical Classifier - Reading Data
category: Artificial Intelligence
---


We have [previously](/naive-bayes-cat-intro/) gone over the basics of the <i>Naive Bayes Classifier</i>. Now it is time to delve into the implementation of the theory.

We will break the implementation into two parts. First we will read the data from a file (a text file in this example for simplicity), extracting all the information we need, and then we will use this information to predict classes. This is Part One.

Example data sets can be found <a href="https://github.com/MrDupin/Machine-Learning/tree/master/Classifiers/Naive%20Bayes/Categorical">here</a>.

<u>This is the preliminaries to the Naive Bayesian Classifier, extracting and storing the data for use in the main algorithm. If you already know how to calculate this data, feel free to skip this. We are simply reading from the file and calculating all the probabilities.</u>

### Overview

#### Classes

For the algorithm to run, we need to know how many items are in a class and how many items in the class have certain features. We will store this information in a dictionary, <i>Classes</i>. The keys to the dictionary are the names of the classes. This dictionary will hold another dictionary, with just one value, for the total of items in the class. The key for this second dictionary is "Total". Lastly, the <i>Classes</i> dictionary will hold dictionaries, of one value, for every feature (and their complements). These dictionaries will hold the number of items in the class that also have the particular feature.

To get the total of items in a class <i>c</i>, and the items in the class that have a feature <i>f</i>, we write:

<pre>
total = Classes[c]["Total"];
withFeature = Classes[c][f];
</pre>

To better understand this dictionary, let's take a look at the following example:

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
</table>

<pre>
total = Classes["Detective"]["Total"]; #total = 20
withFeature = Classes["Brute"]["Slim"]; #withFeature = 5
</pre>

#### Features

We will need another dictionary, to hold information about the features. This dictionary will be named <i>Features</i> and the keys to this are the names of the features. The value in the elements is the number of items (across all classes) that have that feature. To get the total of feature <i>f</i> we write:

<pre>
total = Features[f];
</pre>

For example:

<table>
  <tr>
    <th>Class</th>
    <th>Tall</th> 
    <th>Slim</th>
    <th>Smart</th>
  </tr>
  <tr>
    <td>Detective</td>
    <td>10</td>
    <td>10</td>
    <td>15</td>
  </tr>
  <tr>
    <td>Brute</td>
    <td>25</td>
    <td>5</td>
    <td>5</td>
  </tr>
  <tr>
    <td>Total</td>
    <td>35</td>
    <td>15</td>
    <td>20</td>
  </tr>
</table>

<pre>
total = Features["Smart"]; #total = 20
</pre>

#### Probabilities

The final dictionary we need is to hold all the probabilities we will come across in the algorithm. We will name this dictionary <i>P</i> and the keys will be the name of the probability stored. For example, <i>P["Detective"]</i> is the probability of a detective appearing.

We will need to store the probabilities for classes, features and the conditional probability of features appearing given a certain class.

Note: The probabilities of the classes (P[c]) are often called <i>a priori</i> probabilities.

### Code

#### Read from File

For this example, we have saved the data set in a text file (for simplicity reasons, usually you will find it in a csv). The format is the following:

<pre>
Classes Feature1 Feature2
Class1 Value1 Value2
Class2 Value3 Value4
</pre>

In the first line of the file we begin with the word "Classes". Then, each a space apart, the feature names. Here we do not write the complements of features; We will manually calculate them later. Below the first line, we have the data. Each item is separated by a newline. First comes the class the item is classified in, then the values for the features, each separated with a single space.

In this example we will make the following simplification: the values of the features are simple True/False values, denoting whether an item has a certain feature or not.

We need to store the items in our program. As each item is separated by a newline, we need to split the input by newlines. We will store the resulting array into a temp array, <i>lines</i>.

<pre>
f = open(fileName, 'r');
lines = f.read().splitlines();
f.close();
</pre>

We will also need the number of items in the data set. Remember, line = item, so the length of <i>lines</i> minus 1 is the number of items. To get the names of the features, we will split the first line on the spaces, removing the "Classes" keyword (feature names are stored in the first line of the file, after the "Classes" word). We will also store the number of features for later use. Lastly, we will extract the class data. For this, we will store <i>lines</i> without the first line.

<pre>
#Extract the features
features = lines[:1][0]; #The first line of input
features = features.split(' ')[1:];
l = len(features);

#Extract the class data
classes = lines[1:]; #Remove the first line
</pre>

#### Classes and Features

First we create the two dictionaries to save Classes and Features:

<pre>
Classes = {};
Features = {};
</pre>

Then we will initialize <i>Features</i>. The keys are the names of the features and the value is the number of items in that feature. We will also initialize entries for the complements of the features too.

<pre>
for f in features:
    #For every string in the first line, add a new item
    #to Features, plus its complement.
    Features[f] = 0;
    Features["Not " + f] = 0;
</pre>

Next we will built <i>Classes</i>. We will iterate through the items, which are stored in <i>classes</i>. Each item in that list is a line, with class name and feature values separated by spaces. We will split the line on the spaces. The new array we get has the name of the class on the first element and the feature information in the rest.

We will check if the class name in the item has been added to <i>Classes</i>. If not, we need to add it. For that, we initialize its total and feature counters to 0.

Regardless of whether the class name existed before or not, we increase its total.

Then for each feature <i>f</i> we will check whether the item has it, by checking if the corresponding value is True or False. If it exists, we will increase the counter of Feature <i>f</i>, otherwise we will increase the counter of Feature <i>not f</i>.

If <i>f</i> hasn't been added to the features of the class, we add it now. If it exists, we will increment the number of items in the class who also have the feature.

In code:

<pre>
#Construct Classes table
for c in classes:
    #Split current line (item) by spaces
    #The first element holds the name of the class
    #The rest show whether the item has a certain feature
    c = c.split(' ');

    if(c[0] not in Classes):
        #The item class has not been added to Classes. Add it.
        Classes[c[0]] = {"Total":0}; #Set the class total to 0.
        for f in Features:
            #Add to the class dictionary
            #all the features, set to 0.
            Classes[c[0]][f] = 0;

    #Increment the total items in the item class
    Classes[c[0]]["Total"] += 1;

    for i in range(1, l):
        if(c[i] == 'True'):
            #The item has the feature in the ith
            #index in the item list, c
            #The ith index in c corresponds with
            #the i-1 index in features
            feature = features[i-1];
        elif(c[i] == 'False'):
            #The item doesn't have the feature in the item list
            #Instead, it has the "Not Feature",
            #the complement of the feature
            #Save complement in feature
            feature = "Not " + features[i-1];

        Features[feature] += 1;

        if(feature not in Classes[c[0]]):
            #The feature has not been added to the class
            #dictionary.
            #Add feature to the item class.
            Classes[c[0]][feature] = 1;
        else:
            #The feature exists in the class dictionary.
            #Increment the feature counter in the item class.
            Classes[c[0]][feature] += 1;
</pre>

#### Probabilities

First we create the dictionary for probabilities:

<pre>
P = {};
</pre>

To calculate the probabilities of the classes, we must for each class divide the number of items in the class by the the total of all items.

<pre>
for c in Classes:
    P[c] = Classes[c]["Total"] / float(n);
</pre>

Similarly we calculate the probabilities of features:

<pre>
for f in Features:
    P[f] = Features[f] / float(n);
</pre>

We need to also calculate the probability of a feature appearing, given a class appeared. As a class is given, we no longer use the whole set of items. Instead, we only use the items in the class. Of these items, how many have the feature?

The number of items in a class <i>c</i> is given by <i>Classes[c]["Total"]</i>. The number of items in a class with a feature <i>f</i> is given by <i>Classes[c][f]</i>.

For each class we will calculate the probability of each feature appearing, <i>P[f \| c]</i>. The key is <i>f</i> then '\|' and finally <i>c</i> concatenated. In code:

<pre>
for c in Classes:
    for f in Features:
        P[f+'|' + c] = Classes[c][f] / float(Classes[c]["Total"]);
</pre>

### The End

As we will want to use this code in our classifier script, we will save it in a separate script, to keep our code as concise and clean as possible. So, put all the code above in a function named <i>Read</i>, which will receive as input the name of the file the data set is stored in. At the end of this function we will return the dictionaries we created here (Classes, Features, P). The code in the end will look like this:

<pre>
def Read(fileName):
    #Above code goes here

    return (Classes, Features, P);
</pre>

To call this from another function and get the dictionaries, we will write this:

<pre>
data = DataReader.Read(fileName);
Classes = data[0];
Features = data[1];
P = data[2];
</pre>

To get a better feel of how the dictionaries are structured, print them.

This concludes the extraction of information from the data set. Next we will see how to implement the algorithm itself, which will ran on the data set here.

---

The links to the rest of the series:

[Part 1](/naive-bayes-cat-intro/)

[Part 3](/naive-bayes-cat-implementation/)
