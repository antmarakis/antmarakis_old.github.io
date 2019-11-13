---
layout: post
title: K-Nearest Neighbors
category: Artificial Intelligence
---

We are given a data set of various items with their values, and we want to be able to classify a new item given to us based on that data set (often called <i>training set</i>). A simple algorithm that does that is the <i>k-Nearest Neighbors</i> algorithm (kNN for short).

In a nutshell, it classifies an item depending on the items in the data set closer to it.

### Intro

Say we are given a data set of items, each having numerically valued features (like Height, Weight, Age, etc). If the count of features is <i>n</i>, we can represent the items as points in an <i>n</i>-dimensional grid. Given a new item, we can calculate the distance from the item to every other item in the set. We pick the <i>k</i> closest neighbors and we see where most of these neighbors are classified in. We classify the new item there.

So the problem becomes how we can calculate the distances between items. The solution to this depends on the data set. If the values are real we usually use the Euclidean distance. If the values are categorical or binary, we usually use the Hamming distance.

The algorithm in pseudocode:

<pre>
Given a new item:
     Find distances between new item and all other items
     Pick k shorter distances
     Pick the most common class in these k items
     That class is where we will classify the new item
</pre>

### Reading Data

Let our input file be in the following format:

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

Each item is a line and under "Class" we see where the item is classified in. The values under the feature names ("Height" etc.) is the value the item has for that feature. All the values and features are separated by commas.

You can find two example data file on my Github, <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/kNN/data.txt">here</a> and <a href="https://github.com/MrDupin/Machine-Learning/blob/master/Classifiers/kNN/data2.txt">here</a>. Choose one and paste the contents as is into a text file named <i>data</i>.

We will read from the file (named "data.txt") and we will split the input by lines:

<pre>
f = open('data.txt', 'r');
lines = f.read().splitlines();
f.close();
</pre>

The first line of the file holds the feature names, with the keyword "Class" at the end. We want to store the feature names into a list:

<pre>
#Split the first line by commas, remove the first element
#and save the rest into a list.
#The list now holds the feature names of the data set.
features = lines[0].split(',')[:-1];
</pre>

Then we move onto the data set itself. We will save the items into a list, named <i>items</i>, whose elements are dictionaries (one for each item). The keys to these item-dictionaries are the feature names, plus "Class" to hold the item class. At the end, we want to shuffle the items in the list (this is a safety measure, in case the items are in a weird order).

<pre>
items = [];

for i in range(1, len(lines)):
    line = lines[i].split(',');

    itemFeatures = {"Class" : line[-1]};

    for j in range(len(features)):
        #Iterate through the features
        f = features[j];
        #The first item in the line is the class, skip it
        v = float(line[j]);

        itemFeatures[f] = v;
    
    items.append(itemFeatures);

shuffle(items);
</pre>

### Classifying

With the data stored into <i>items</i>, we now start building our classifier. For the classifier, we will create a new function, <i>Classify</i>. It will take as input the item we want to classify, the items list and <i>k</i>, the number of the closest neighbors.

If <i>k</i> is greater than the length of the data set, we do not go ahead with the classifying, as we cannot have more closest neighbors than the total amount of items in the data set. (alternatively we could set k as the <i>items</i> length instead of returning an error message)

<pre>
if(k > len(Items)):
        return "k larger than list length";
</pre>

We want to calculate the distance between the item to be classified and all the items in the training set, in the end keeping the <i>k</i> shortest distances. To keep the current closest neighbors we use a list, called <i>neighbors</i>. Each element in the least holds two values, one for the distance from the item to be classified and another for the class the neighbor is in. We will calculate distance via the generalized Euclidean formula (for <i>n</i> dimensions). Then, we will pick the class that appears most of the time in <i>neighbors</i> and that will be our pick. In code:

<pre>
def Classify(nItem, k, Items):
    if(k > len(Items)):
        return "k larger than list length";
    
    #Hold nearest neighbors.
    #First item is distance, second class
    neighbors = [];

    for item in Items:
        #Find Euclidean Distance
        distance = EuclideanDistance(nItem, item);

        #Update neighbors,
        #either adding the current item in neighbors or not.
        neighbors = UpdateNeighbors(neighbors, item, distance, k);

    #Count the number of each class in neighbors
    count = CalculateNeighborsClass(neighbors, k);

    #Find the max in count,
    #aka the class with the most appearances.
    return FindMax(count);
</pre>

The external functions we need to implement are <i>EuclideanDistance</i>, <i>UpdateNeighbors</i>, <i>CalculateNeighborsClass</i> and <i>FindMax</i>.

### Euclidean Distance

<pre>
def EuclideanDistance(x,y):
    S = 0;
    for key in x.keys():
        S += math.pow(x[key]-y[key], 2);

    return math.sqrt(S);
</pre>

### Update Neighbors

We have our neighbors list (which should at most have a length of <i>k</i>) and we want to add an item to the list with a given distance. First we will check if <i>neighbors</i> has a length of <i>k</i>. If it has less, we add the item to it irregardless of the distance (as we need to fill the list up to <i>k</i> before we start rejecting items). If not, we will check if the item has a shorter distance than the item with the max distance in the list. If that is true, we will replace the item with max distance with the new item.

To find the max distance item more quickly, we will keep the list sorted in ascending order. So, the last item in the list will have the max distance. We will replace it with the new item and we will sort again.

To speed this process up, we can implement an Insertion Sort where we insert new items in the list without having to sort the entire list. The code for this though is rather long and, although simple, will bog the tutorial down.

Code:

<pre>
def UpdateNeighbors(neighbors, item, distance, k):
    if(len(neighbors)  distance):
            #If yes, replace the last element with new item
            neighbors[-1] = [distance, item["Class"]];
            neighbors = sorted(neighbors);

    return neighbors;</pre>

### Calculate Neighbors Class

Here we will calculate the class that appears most often in <i>neighbors</i>. For that, we will use another dictionary, called <i>count</i>, where the keys are the class names appearing in <i>neighbors</i>. If a key doesn't exist, we will add it, otherwise we will increment its value.

<pre>
def CalculateNeighborsClass(neighbors, k):
    count = {};
    
    for i in range(k):
        if(neighbors[i][1] not in count):
            #The class at the ith index is not in the count dict.
            #Initialize it to 1.
            count[neighbors[i][1]] = 1;
        else:
            #Found another item of class c[i].
            #Increment its counter.
            count[neighbors[i][1]] += 1;

    return count;
</pre>

### Find Max

We will input to this function the dictionary <i>count</i> we build in <i>CalculateNeighborsClass</i> and we will return its max.

<pre>
def FindMax(countList):
    maximum = -1; #Hold the max
    classification = ""; #Hold the classification
    
    for key in countList.keys():
        if(countList[key] > maximum):
            maximum = countList[key];
            classification = key;

    return classification, maximum;
</pre>

### Conclusion

With that this kNN tutorial is finished.

You can now classify new items, setting <i>k</i> as you see fit. Usually for <i>k</i> an odd number is used, but that is not necessary. To classify a new item, you need to create a dictionary with keys the feature names and the values that characterize the item. An example of classification:

<pre>
newItem = {'Height' : 1.74, 'Weight' : 67, 'Age' : 22};
print Classify(newItem, 3, items);
</pre>

You can find the complete code on <a href="https://github.com/MrDupin/Machine-Learning/tree/master/Classifiers/kNN">my GitHub</a> (the code includes a Fold Validation function, but it is unrelated to the algorithm, it is there for calculating the accuracy of the algorithm).
