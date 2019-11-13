---
layout: post
title: Perceptron Theory
category: Artificial Intelligence
---

The Perceptron is a supervised linear classifier, combining weights with a feature vector to classify an item. Given a dataset of items, we train the Perceptron (by adjusting the weights) so that we can classify new items in the future.

### Classifying

In the general case, with multiple classes, we need a matrix of the weight vectors (each for each class). Then, given an item with a feature vector of <b>x</b>, we calculate its dot product with each weight vector and we pick the maximum. The item is classified in that class. In a formula:

$$max{ w_i \* x + b_i }$$

Where \(( w_i \\) and \(( b_i \\) the weight vector and bias of a class respectively. The bias is not necessary, but usually is helpful (for some math-y reasons I won't get into). The bias does not depend on the input vector. Because it is not elegant to have to work with another variable, we will <i>augment</i> the vectors to include the bias. That means we will add to the weight and feature vectors another element. For the weight vectors, we will "stick" the bias value at the end of the vectors. So, a weight vector for class <i>i</i> and <i>f</i> features now becomes:

$$[w_{i,1}, w_{i,2}, ... , w_{i,f}, b_i]$$

For the feature vector, we augment it with a 1 (because when we multiply the weight and feature vector, we get \(( b_i \\) times 1, for the bias).

<p align="center">[x_{i,1}, ... , x_{i,f} , 1]$$

Back on the dot product. In this tutorial, we can call it *confidence*, as it is an indicator of how confident we are that we picked the right class over other classes.

### Training

We train the Perceptron so it can "learn" to classify items by adjusting the augmented weight vectors.

When we make a prediction that is not correct, we need to learn from that mistake and correct our weight matrix as best as possible. Say the correct classification for an item was <i>g</i> but we classified it in <i>h</i>. We need to adjust the weight vectors where the mistake happened.

If we imagine those two vectors as lines that "cut" the space into parts, we want the item to fall under the part that was "cut" by the vector <i>g</i>. Currently though it falls under the <i>h</i> one instead. To fix this, we will move the <i>g</i> one towards the item and the <i>h</i> one away from it. We do that by adding the item feature vector to the first one and subtracting it from the second.

To minimize the radicality of this action, we will implement a <i>learning rate</i> to these arithmetics to have better control on the outcome.

More formally, when we make an error in the categorization, we do the following:

For the correct class -

$$w_g += r\*x$$

For the guessed/wrong class -

$$w_h -= r\*x$$

With the above we fixed the weight functions for the given item, so if we were to re-classify the item, we would probably classify it under the correct class.

What about the other items in the data set? We changed the weight vectors, so there is a high chance we messed it up for some other items. So we need to go back and fix that for them too. Then we might again mess it up for some other items and the cycle continues.

In fact, if the classes aren't linearly separable (which means they can't be separated by a line), the cycle can continue forever.

To combat this infinite loop, we have many options. One is to stop after the amount of corrections becomes small and the weight vectors don't change much. Another, is to stop once you hit a certain threshold of accuracy.

The most popular solution though is to loop through the items a fixed amount of times. Each loop is called an <i>epoch</i>. After the epochs are finished, we take the weight matrix that was produced.

### Pseudocode

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

### Closing Notes

In this post I touched on the multi-class Perceptron, which can also be referenced as the <i>Kesler's Construction</i> Perceptron. In the binary class case (where there are only two classes) the algorithm is simplified, by using only a single weight vector instead of a matrix of weight vectors. In that case, we do not pick the maximum dot product of the weight and the feature vector, but instead we check if the dot product is greater or less than a threshold. Above the threshold it is classified in class A, below the threshold it is classified in class B.

Another variation is the <i>Pocket Algorithm</i>. As we go through the epochs, we keep (in a "pocket") the best solution we have found yet. If we find a solution with better accuracy, we throw out the one in our pocket and add the new one. We continue until we finish our epochs.

With that, this little introduction is finished.

<hr>

[Perceptron Implementation](/perceptron-implementation/)
