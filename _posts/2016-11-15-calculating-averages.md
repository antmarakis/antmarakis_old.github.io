---
layout: post
title: Calculating Averages - A Bit of Info
category: Bits of Info
---

Let's take a look at the very popular problem of finding the average of a list of numbers. The conventional method for this is to add all the numbers and then divide by the length of the list. For example:

<pre>
1, 3, 5, 7

1 + 3 + 5 + 7 = 16
16/4 = 4

So the average is 4.
</pre>

Now let's say you add another number to the list, 9. What would you do? If you follow the above method, you will re-add all the numbers and divide again. Here that's not a problem, but imagine you have a huge list with a lot of numbers coming in. Would you still add all the numbers and then divide?

That's wasteful. We need to find a better algorithm for finding the average. To do that, we will need to use the previous average to calculate the new average. We will name the previous average <i>prevAvg</i>, the previous list length <i>prevLength</i>, the current list length <i>currLength</i> and the new number <i>x</i>. We want to find the current average, <i>currAvg</i>.

<p align="center">currAvg = (prevAvg*prevLength + x) / currLength</p>

In Python code:

<pre>
#Initial list, average and length
sequence = [1,3,5,7];
avg = 4;
length = 4;

#Add x to list, length increases by 1
x = 9;

avg = (avg*length+x)/(length+1);
print(avg);
</pre>

This can speed up a lot your work, especially when you are dealing with big data and want to calculate, say, the mean.
