---
layout: post
title: Dynamic Programming - Rod Cutting
category: Dynamic Programming
---

We are given a rod of length <i>n</i> and we want to sell it. In order to sell it, we can either sell it as is or cut it into pieces and sell them separately. The selling price is directly dependent on the length of the piece.

The only restriction is that we can only cut the rod into pieces of integer length.

We are also given an array of prices, also of length <i>n</i>. This array contains the price for selling a rod at a certain length. For example, <i>prices[5]</i> shows the price we can sell a rod of length 5. Generalising, <i>prices[x]</i> shows the price a rod of length <i>x</i> can be sold.

We are tasked to find the optimal solution to sell the given rod and prices array.

### Approach

When we receive a rod of length <i>n</i>, we have two options:

a) Don't cut it and sell it as is (receiving <i>prices[n]</i>)<br>
b) Cut it and sell it in two parts. One part the length we cut it and the other the rod we are left with, which we have to try and sell separately. How though are we cutting the rod? We are cutting it in two parts, so one will have length, say, <i>i</i> and the other length <i>n-i</i>. We need to check which cut gives the greatest profit of all the possible cuts.<br>
c) Choose the maximum price of the two.

The function representing our approach is the following:

<p align="center">Profit(n) = Max{ Price(n), Price(i) + Profit(n-i) }</p>

Now we need to find our base case. For that, we will look at our restriction. We can only cut our rod in integer bits. So the smallest length we can get is 1. One could say that the smallest length of a rod is 0, but that case is not possible as rod of length 0 means that we don't have a rod.

So, if we get a rod of length 1, we can only sell it as is. The profit of this sale is <i>prices[1]</i>.

We have developed the method we will follow and we have found our base case, so we can head right into the code.

### Implementation

First, we need to initialize the array that will hold the solutions for the different lengths. The values of this array will be initialized at -1 and we will update them every time we find a better solution. We chose -1 so that we can easily check if we have found a solution for the given length.

Moreover, we will need our array of prices. The first value (for index=0) should be 0, as we cannot sell for profit a rod of no length. You can set the rest to any value you want, in any order. Ascending order makes intuitive sense; as the length increases so does the price. You don't have to follow intuition though; be a rebel! (although another order might produce weird or trivial results)

Lastly, we need to print the solution. For that, we call the function <i>SellRod(n)</i>, which we will develop later.

For the above, we have the following code:

<pre>
prices = [0, 1, 5, 8, 9, 10, 17, 17, 20, 24, 30];
solutions = [-1 for x in range(length+1)];

print SellRod(n);
</pre>

With that out of the way, we can move to the main bulk of the algorithm.

1) We check for the base case.
2) We calculate the profit if we sell the rod without cutting it any further.
3) We calculate the profit if we cut the rod at all possible lengths.
   *Also, we check if we have already computed a solution. If not, calculate it.
4) Find the max of steps 2) and 3).
5) Return max.

The code is given below.

<pre>
def SellRod(n):
    if(n == 1):
         #base case
         return prices[1];

    #The profit if you sell the rod as is  
    noCut = prices[n];
    #The prices for the different cutting options, set to -1.
    yesCut = [-1 for x in range(n)];

    for i in range(1, n):
        if(solutions[i] == -1):
            #We haven't calulated solution for length i yet.
            #We know we sell the part of length i,
            #so we get prices[i].
            #We just need to know how to sell rod of length n-i.
            yesCut[i] = prices[i] + SellRod(n-i);
        else:
            #We have calculated solution for length i.
            #We add the two prices.
            yesCut[i] = prices[i] + solutions[n-i];
        
        #We need to find the highest price in order
        #to sell more efficiently.
        #We have to choose between noCut and
        #the prices in yesCut.
        maxProfit = noCut; #Initialize max to noCut
        for i in range(n):
            if(yesCut[i] > maxProfit):
                maxProfit = yesCut[i];
        
        solutions[n] = maxProfit;
        return maxProfit;
</pre>
