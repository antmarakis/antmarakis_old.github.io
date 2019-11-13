---
layout: post
title: Perceptron Implementation
category: Artificial Intelligence
---

In a [previous post](/perceptron-theory/) we looked at the theory behind Perceptron. In this tutorial we will implement the algorithm using Python.

<u>Note:</u> We will implement the algorithm from scratch, so that we get a better understanding of how the algorithm actually works. If you want to use it to solve a problem, it's better if you used an external library that already implements the algorithm (like scikit-learn for Python).

### Read Data

*Feel free to skip this section if you don't care how to read data from a file.*

First we need to read the data from a file. Each line in the file represents an item, and the first line holds the feature names (+ the keyword "Class" at the end). The file follows this format:

<pre>
Height,Weight,Age,Class
1.70,65,20,Programmer
1.90,85,33,Builder
1.78,76,31,Builder
1.73,74,24,Programmer
1.81,75,35,Builder
1.73,70,75,Scientist
1.80,71,63,Scientist
1.75,69,25,Programmer
</pre>

You can find an example data file <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/Perceptron/data.txt">here</a>.

Let's jump right into the implementation. We need to get the number of features, which can be found on the first line.

<pre>
featuresNumber = len(lines[0].split(','));
</pre>

We also need to store the item information and the different classes that appear in the data set. For that, we will use three lists, <i>items</i>, <i>classes</i> and <i>features</i>. The second and third ones will hold the names of the classes and features respectively. The first one is a bit more complicated though. It will hold dictionaries, with each dictionary representing an item. The keys to these dictionaries are the feature names and will hold the values the item has for the feature/key, "Class" to hold the class the item is categorized in, and "Bias" which will hold a value of 1. With Bias we are essentially augmenting the item/feature vector with a value of 1.

For each line in the file, we will split it to get the features, storing them into a temporary array before adding them to the <i>items</i> list. At the end, we will shuffle the list to make sure the data set is properly randomized.

<script src="https://gist.github.com/MrDupin/e1d643d6bc9799424c2f4642ddd083c0.js"></script>

### Auxiliary Functions

First we will need functions to add and subtract dictionaries (<i>d1</i> and <i>d2</i>). We operate at a rate (eg. instead of subtracting the whole of <i>d2</i>, we subtract its third), which is given as a parameter.

We also need a function to calculate the confidence (the dot product) of an item and a weight. We can calculate the dot product by multiplying the values for each feature of the weight and item vectors and adding them together.

<script src="https://gist.github.com/MrDupin/b4a684dc2229c060e163cc55a2ac8dd2.js"></script>

### Training

Now we move onto the meat of the algorithm.

We will create the function to train the perceptron (aka calculating the weights). We will name this function `CalculateWeights`. It takes as input the training data, the learning rate (otherwise known as *correction rate*), the classes and features, and the number of epochs (aka how many times we will loop through the data).

First we want to initialize the weights. As we are building a mulit-class Perceptron using Kesler's Construction, each class has its own weights for the features. To implement this, we will use a dictionary of dictionaries. The keys of the outer dictionary are the classes and for each class-dictionary, the keys are the features. We will initialize the values of each weight to 0.

<pre>
weights = {};

for c in classes:
    weights[c] = {"Bias":0};
    for f in features:
        weights[c][f] = 0;
</pre>

To get a better feel of how the dictionary <i>weights</i> is laid out, we will quickly read data from it:

<pre>
print weights[c][f]; #Prints the weight of feature f for class c.
print weights[c]; #Prints the feature weights for class c.
</pre>

Having initialized our weights, we move to adjusting them. The algorithm in pseudocode:

<pre>
For each epoch:
    For each item i in training set:
        Find confidence of i for all classes
        Pick max confidence
            c is the class corresponding to max confidence

        Compare c with actual class of i
        If they are the same, the guess was correct
            Move to next item
        If they are different, do the following:
            For the weights of c, subtract the item values
            For the weights of the correct class, add item values
</pre>

Let's implement the above:

<script src="https://gist.github.com/MrDupin/9f82fcd944fd12394797ac26829be0d8.js"></script>

Bringing all the parts of the function together we have:

<script src="https://gist.github.com/MrDupin/c7929c4baadac8ab7fa6c383e9fbf990.js"></script>

With the above function we can train the weights, now we need to write a function to classify an item. We will call this function <i>Perceptron</i>. Given an item and the weights, first it will augment the item vector with the bias, then it will find the maximum confidence and it will classify the item in the class that produced that max confidence.

<script src="https://gist.github.com/MrDupin/eb330097fa36748396b6cfbf2d0c4a5a.js"></script>

<h3>Usage</h3>

To use the Perceptron we will first read our data from a text file, we will set the parameters of the Perceptron (often called hyper-parameters and they are the learning rate, epochs, etc.) then we will calculate the weights and finally we will classify the new item.

<pre>
items, classes, features = ReadData('data.txt');

lRate = 0.1;
epochs = 300;

item = {'PW' : 1.4, 'PL' : 4.7, 'SW' : 3.2, 'SL' : 7.0};
print Perceptron(item,weights);
</pre>

With that the code is complete. You can find the complete implementation on <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/Perceptron/Perceptron.py">my github</a>. It includes two evaluation functions.

<hr>

[Perceptron Theory](/perceptron-theory/)
