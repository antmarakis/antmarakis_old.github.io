---
layout: post
title: Divide and Conquer - Find Majority Element
category: Divide and Conquer
---

We are given an array and we want to find its majority element, if it has one, using a divide-and-conquer algorithm in O(n*log.n).

A majority element of an <i>n</i>-sized array is defined as an element that appears more than <i>n/2</i> times.

### Solution

We will keep dividing the array into half until we reach an array of size two and we will compare the two elements of each array. If they are the same, they are the majority element of that array and we will return their value. If they are not the same, we will return a special character to signify that there is no majority element.

Moving recursively up the arrays, we will check the values we get from the two child-arrays/halves. As with the base case above, if the elements are the same, we return them, otherwise we return the special character.

Essentially we are checking two elements each time and if they are the same we keep them, while otherwise we discard them.

This is a simple example of a "Divide and Conquer" algorithm which showcases how to use the particular technique to our advantage. We have a big problem which will be easier to solve if we break it up into smaller problems. We keep breaking the problem up until we get to a base case (here the 2-element array) which we can trivially solve and then recursively propagate the solution up until we solve the original problem.

You can find the code <a href="https://github.com/MrDupin/Algorithms/blob/master/Divide%20and%20Conquer/FindMajorityElement.py">here</a>.

NOTE 1: This particular problem also has an O(n) solution without the use of divide-and-conquer. Can you find it?

NOTE 2: Upon revisiting this algorithm (after someone brought this to my attention), I have found an issue. This will not work for arrays in some particular forms (for example: [1, 1, 2, 2, 2] - can you see why?). Since the only issue is the order of the array, simply shuffling it will most likely suffice.
