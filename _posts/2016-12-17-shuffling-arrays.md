---
layout: post
title: Shuffling an Array Without Shuffling It - A Bit of Info
category: Bits of Info
---

This is a quirky way to shuffle an array, without actually rearranging it. The basic idea is that we shuffle the order of the indices of the array, instead of the items themselves. Then we will iterate through the order of the shuffled indices and use them as an index to get the next item in the array. Without further ado, let's jump straight into the code.

<h3>Algorithm:</h3>

We have our array:

<pre>
array = [5, 17, 23, 1, 11, 99, 37];
</pre>

First we find its length and we initialize a new array, called <i>order</i>, that will hold the indices of the array, from 0 to n-1.

<pre>
n = len(array);

order = range(n);
</pre>

In this case, <i>order</i> is [0, 1, 2, 3, 4, 5, 6].

We want to shuffle the indices in <i>order</i> around.

<pre>
order = Shuffle(order);
</pre>

Where <i>Shuffle</i> is a function we will build on our own. First thing in the function, we will get the length of <i>order</i>. Then we will iterate through the indices in <i>order</i>, and for every index we will pick a random index and we will swap them in place.

<pre>
def Shuffle(order):
        n = len(order);

       for i in range(n):
               r = randint(0, n-1);

               order[i], order[r] = order[r], order[i];
</pre>

Where <i>r</i> is the random index to swap with. To use <i>randint</i> we will need to import it from the <i>random</i> library.

<pre>
from random import randint;
</pre>

With the function done, we can return to our main code. We just shuffled <i>order</i>. What we do next is up to the program we want to make. The general idea is that we will iterate through the indices in <i>order</i> and use the iterated index to get an item from the array.

For the purpose for this tutorial, say we want to simply print the numbers in the array in a random order. We write this:

<pre>
for i in range(n):
        index = order[i];
        print(array[index]);
</pre>

Instead of the <i>print</i> command, you can use the array as you wish.

<h3>Conclusion</h3>

This algorithm can be used in mainly two cases:

1) The items in array are large and would be costly (time wise) to swap around.
2) You want the initial array to remain intact, without duplicating (because it might be costly, memory wise).
