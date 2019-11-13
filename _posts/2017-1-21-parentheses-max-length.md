---
layout: post
title: Algorithms - Parentheses Max Length
category: General Algorithms
---

In this tutorial we will take a look at a problem which falls under the general category of algorithmic problems without the need of a particular design technique to get a good solution.

### Problem

We are given a string of parentheses and we want to find the maximal length of substrings. For example, for the string "())(())" the maximal length is 4 and comes from the substring "(())".

### Algorithm Analysis

We will calculate the maximal length of valid parentheses by matching closing parenthesis with opening parenthesis.

We are utilizing a stack to hold the indexes of parentheses. The first index indicates where the current string of parentheses starts. If the first index is <i>k</i> and the last index is <i>m</i>, then the string currently in the stack is the string from <i>k+1</i> to <i>m</i> (the first index is <u>not</u> in the string).

The algorithm then is as follows:

<pre>
We iterate through the given string of parentheses:
If we encounter an opening parenthesis, we push it on the stack.
If we encounter a closing parenthesis, we do the following:
   We check if the stack is empty:

     -If not, we have found a valid substring.
     We check if it is the max found yet.
     To do that, we need to find its length.
     The length of the current substring
     is the difference between the current index
     and the last index in the stack
     (since the valid string is between those two indexes).
     If that is larger than the maximum length
     we have found previously, we update it.

     -If the stack is empty,
     we push the current index on its top. The next (possible)
     valid string begins after it.

If we encounter a character other than '(' or ')', we return -1.

The final value of maxLength is the solution.
</pre>

### Complexity Analysis

* Iterating over the given index: O(n), since we are making one linear pass.
* Pushing and popping are O(1).
* The rest are O(1) calculations.

* Thus the algorithm is O(n). ( because O(n)*O(1)*O(1) = O(n) )
* Since the algorithm is O(n) and for a solution we have to read the whole string, which takes O(n), the algorithm is optimal.

### Code
<pre>
def FindLength(par):
    parLength = len(par);
    maxLength = 0;
    stack = [-1];
    
    for i in range(parLength):
        p = par[i];
        if(p == '('):
            #Add index to stack
            stack.append(i);
        elif(p == ')'):
            stack.pop(); #Pop top item
            stackLength = len(stack);
            if(stackLength > 0):
                #Stack is not empty, calculate maxLength
                lastIndex = stack[-1];
                if(i-lastIndex > maxLength):
                    maxLength = i-lastIndex;
            else:
                #Stack is empty, set it to current index
                stack = [i]
        else:
            #Encountered char other than parentheses
            return -1;

    return maxLength;
</pre>

For testing:

<pre>
par = "()(())";
print FindLength(par);
par = "(((()()))";
print FindLength(par);
</pre>

---

You can also find the code on <a href="https://github.com/MrDupin/Algorithms/blob/master/General/Parentheses_MaxLength.py">my GitHub</a>. Thanks for dropping by.
