---
layout: post
title: Dynamic Programming - Maximum Subarray Sum
category: Dynamic Programming
---

Given an array <i>S</i> of length <i>n</i> and integer values, we want to find the maximum subarray sum in <i>S</i>. For example, if S = [1, -2, 3, 4, 5, -6], the maximum subarray sum is <i>3+4+5=12</i>.

### Approach

To solve this problem with a Dynamic Programming approach, we need to consider whether we can use solutions to subproblems to help us in the main problem.

Define <i>Sum(i)</i> as the maximum subarray sum we can get at index <i>i</i>. In a sense, <i>Sum(i)</i> is the maximum sum we can get from a subarray ending at <i>i</i>. That way we can use this solution to bigger problems. It is worth noting that this solution is just a number, the maximum sum. If we want to know the subarray too, we would need to extend the solution. (In this example we will not implement the algorithm this way.)

Say we want to compute the solution to the problem of length <i>n</i>. Let <i>Sum(n-1)</i> be the maximum sum ending with <i>S[n-1]</i>. We know we must pick <i>S[n]</i>, so the question is whether we should add <i>Sum(n-1)</i> to the solution or not. Remember, we want to find the maximum subarray sum, so naturally we will pick the max of the two choices. If <i>S[n]</i> >  <i>Sum(n-1) + S[n]</i> then we only need to pick <i>S[n]</i> and leave <i>Sum(n-1)</i> out of the solution. <i>Sum(n)</i> therefore is <i>Sum(n) = Max{Sum(n-1) + S[n], S[n]}</i>

Generalizing, if we want to solve for length <i>i</i>, we have:

<p align="center">Sum(i) = Max{ Sum(i-1) + S[i], S[i] }</p>

Where we are basically either choosing to pick <i>S[i]</i> alone or choosing to pick the solution that came before it too.

To do this, we will loop through the array <i>S</i> and update the values in the array of solutions.

### Implementation

(Using Python)

Our given array <i>S</i>. This array holds any numbers, in this case positive and negative integers.

<pre>
S = [-2, 1, -3, 7, -2, 3, 1, -5, 4];
</pre>

The length of <i>S</i>.

<pre>
n = len(S);
</pre>

The array to hold the solutions.

<pre>
sums = [0 for x in range(n+1)];
</pre>

The function to calculate the solution for <i>n</i> will be called <i>CalculateSubarraySum</i>.

To iterate through the elements is <i>S</i> we employ the below loop. Inside this loop we will calculate the value of <i>sums[i]</i>.

<pre>
for i in range(1, n+1):
	#Here goes the code to calculate sums[i]
</pre>

Remember, <i>sums[i]</i> should have the value of the max between <i>sums[n-1] + S[n]</i> and <i>S[n]</i>. So the loop becomes:

<pre>
for i in range(1, n+1):
    if(sums[i-1] + S[i-1] > S[i-1]):
        #We pick the previous sum, plus S[i]
        sums[i] = sums[i-1] + S[i-1];
    else:
        #We pick S[i]
        sums[i] = S[i-1];
</pre>

Now the array <i>sums</i> holds the solutions for all length from 0 to <i>n</i>. We need to pick and return the maximum in <i>sums</i>, which will also be the optimal solution for our problem.

<pre>
return max(sums)
</pre>

The function in its entirety:

<pre>
def CalculateSubarraySum(n):
    for i in range(1, n+1):
        if(sums[i-1] + S[i-1] > S[i-1]):
            #We pick the previous sum, plus S[i]
            sums[i] = sums[i-1] + S[i-1];
        else:
            #We pick S[i]
            sums[i] = S[i-1];

    return max(sums);
</pre>

To print this value, we call <i>CalculateSubarraySum(n)</i> and print it.

<pre>
print(CalculateSubarraySum(n));
</pre>

With that we are done. You can find the complete code on my <a href="https://github.com/MrDupin/Algorithms/blob/master/Dynamic%20Programming/MaximumSubarraySum/MaximumSubarraySum.py">Github repository</a>.

Thanks for reading!
