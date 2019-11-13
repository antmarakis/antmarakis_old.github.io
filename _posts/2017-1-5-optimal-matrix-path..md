---
layout: post
title: Dynamic Programming - Optimal Path in Matrix
category: Dynamic Programming
---

We are given an <i>nxn</i> matrix. The elements in the matrix are random, positive integers. We want to go from the top left corner to the bottom right by moving only right and down (no left, up or diagonal moves). Our goal is to move using the minimum cost path.

Let our matrix be the following:

<pre>
5   3   10  17  1
4   2   9   8   5
11  12  3   9   6
1   3   4   2   10
7   11  13  7   3
</pre>

### Approach

To solve this problem we will use a Dynamic Programming approach.

Remember, we can only make two moves. That helps a lot in the design of the algorithm. Because we <i>know</i> where the optimal solution will come from. To reach a node we will do so either from its left or its top. Naturally the optimal solution follows this pattern.

Generally, for an element X, the optimal way to get to it from the top left passes through either its left or its top. To reach its left and top, we need paths of costs <i>a</i> and <i>b</i>. We pick the smallest of the two and we have the optimal solution to reach X.

So, to get to the bottom right corner, we need to pick either the left or top cost. To compute those values we need to know <i>their</i> left and top and so on. The base case is the top left element, where we need <i>Matrix(0,0)</i> to get to (as we're starting from there).

So, the formula to find the path cost to get to the element <i>(i,j)</i> is this:

<p align="center">Path(i, j) = min{ Path(i-1, j), Path(i, j-1) } + Matrix(i, j)</p>

We pick the minimum of the left <i>(i,j-1)</i> and top <i>(i-1,j)</i> costs and we add the value of the element. That's the cost of the path to get to <i>(i,j)</i>.

As the matrix has length of <i>n</i>, the cost to get to the bottom right is <i>Path(n, n)</i>.

### Implementation

(Using Python)

First we need our matrix. You can receive the matrix as you wish as long as it is <i>nxn</i>; reading it from a file, reading it from the user's input, or any way you wish. Here we will manually pass the values in the source code.

<pre>
matrix = [[5, 3, 10, 17, 1],
          [4, 2, 9, 8, 5],
          [11, 12, 3, 9, 6],
          [1, 3, 4, 2, 10],
          [7, 11, 13, 7, 3]];
</pre>

We also need to initialize the array that will hold the solutions. This array needs to be the same dimensions as <i>matrix</i>. As <i>matrix</i> is square, we only need to get one of its dimensions. We can write:

<pre>
n = len(matrix);
</pre>

Our array to hold the solutions will be called <i>solutions</i> and will be an <i>nxn</i> array. We initialize it like this (all its values start from 0):

<pre>
solutions = [[0 for i in range(n)] for j in range(n)];
</pre>

For our base case, the first element of the matrix (0,0) will have a solution of <i>matrix[0][0]</i> (as we are starting from there).

<pre>
solutions[0][0] = matrix[0][0];
</pre>

Now we need to take a step back and think about the problem at hand. Is there any way we can make it easier on ourselves? Remember our equation:

<p align="center">Path(i, j) = min{ Path(i-1, j), Path(i, j-1) } + Matrix(i, j)</p>

Think of what will happen on the first collumn and the first row of the matrix. These border cases pose a problem, as the index in Path will get out of range. In the case of the first collumn, <i>j = 0</i> and <i>j-1 = -1</i>, therefore <i>Path(i,j-1)</i> goes out of range. Likewise for the first row.

Even though this looks like an obstacle, we can actually use it to our advantage. Because we already know what the solution to these border cases is. We take out the out-of-range Path and we use the rest of the equation. For the first collumn, the solution therefore becomes:

<p align="center">Path(i, j) = Path(i-1, j) + Matrix(i, j)</p>

And for the first row it is:

<p align="center">Path(i, j) = Path(i, j-1) + Matrix(i, j)</p>

In simple terms, the solution of the first collumn elements is their value plus the solution of the above element. For the first row, the solution of an element is its value plus the solution of the left element. The base case in both is the first element of the array which has a solution of <i>matrix[0][0]</i>. In code:

<pre>
for i in range(1, n):
    solutions[i][0] = solutions[i-1][0] + matrix[i][0];

for j in range(1, n):
    solutions[0][j] = solutions[0][j-1] + matrix[0][j];
</pre>

Now we can work on the main body of the algorithm, the computation of the solution. We will use a recursive function for this. We will call this function <i>CalculatePath</i>. Keep in mind that the recursion will not reach the border cases (first row and first collumn) so we do not need to check range. The index for the row is <i>i</i> and the collumn is <i>j</i>. For example, <i>i=3, j=5</i> means the 5th element on the 3rd row.

Let <i>(i,j)</i> be the current indexes.

The main idea behind Dynamic Programming is the use of previously computed solutions in the computation of the current solution. We will use this technique here too. In this implementation, previous solutions are stored in the array <i>solutions</i>. We have initialized this array at 0, so every value that is larger than that signifies a solution has been computed. So, every time we reach a solution that is larger than 0, we will return it as is. In code:

<pre>
if(solutions[i][j] > 0):
    #We have already calculated solution for i,j; return it.
    return solutions[i][j];
</pre>

If we have not computed a solution we can use, we need to compute it. We do it by following the general formula we came up with earlier. We need to find the optimal solution for the element on top and for the element on the left of the current element.

<pre>
#Optimal solution for i-1,j (top)
topCost = CalculatePath(i-1, j) + matrix[i][j];

#Optimal solution for i,j-1 (left)
leftCost = CalculatePath(i, j-1) + matrix[i][j];
</pre>

We pick the minimum of the two and we add to it the value of the current element, <i>matrix[i][j]</i>. That is the optimal solution for <i>(i,j)</i>. We store it at <i>solutions</i>.

<pre>
if(topCost < leftCost):
    solutions[i][j] = topCost;
    return topCost;
else:
    solutions[i][j] = leftCost;
    return leftCost;
</pre>

Adding them all together, our recursive function is this:

<pre>
def CalculatePath(i, j):
    if(s[i][j] > 0):
        #We have already calculated solution for i,j; return it.
        return s[i][j];

    #Optimal solution for i-1,j (top)
    topCost = CalculatePath(i-1, j) + matrix[i][j];

    #Optimal solution for i,j-1 (left)
    leftCost = CalculatePath(i, j-1) + matrix[i][j];

    #Store and return the optimal (minimum) solution
    if(topCost < leftCost):
        s[i][j] = topCost;
        return topCost;
    else:
        s[i][j] = leftCost;
        return leftCost;
</pre>

To print the solution, we write:

<pre>
print CalculatePath(n-1, n-1);
</pre>

For the code you can go to <a href="https://github.com/MrDupin/Algorithms/tree/master/Dynamic%20Programming/MinimumPathSum">my Github repository</a> (I read the matrix from a text file).
