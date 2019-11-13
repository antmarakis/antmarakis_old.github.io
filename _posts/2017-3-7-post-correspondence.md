---
layout: post
title: Post Correspondence Problem Exercises
category: Computation Theory
---


*Exercises found in Michael Sipser's "Introduction to Computation Theory"*

---

We will solve three exercises on the *Post Correspondence Problem (PCP)*, which is defined as follows:

We have *n* string pairs, with the following arrangement: There is a top string and a bottom string in the pair (think of it as a domino piece, with a string of chars on the top and bottom). Those can be different or the same and they can be of any length. An example is the following:

$$\dfrac{b}{ca} \dfrac{a}{ab} \dfrac{ca}{a} \dfrac{abc}{c}$$

We want to arrange the pairs in a way that the string created from the concatenation of the top strings is the same as the string created by the bottom strings. We must use each pair, but we can use pairs as many times as we want. Also, we cannot turn the pairs upside down.

In the above example, we can create such an arrangement:

$$\dfrac{a}{ab} \dfrac{b}{ca} \dfrac{ca}{a} \dfrac{a}{ab} \dfrac{abc}{c}$$

Onward to the problems.

*a) Show that the *Post Correspondence Problem* is decidable over the unary alphabet S = {1}.*

We will count the number of 1s in the top strings (*t*) and the number of 1s in the bottom string (*b*). If `t = b`, we accept. Any way we place the pairs, it will always be that the top is the same as the bottom. If `t != b`, we reject.

Example:

$$\dfrac{111}{1} \dfrac{11}{11} \dfrac{1}{11} \dfrac{1}{111} \dfrac{11}{1}$$

The top string has a length of 9 and the bottom also has a length of 9. As the alphabet is unary, the top will be 9 1s and the bottom will also be 9 1s. Any way we place the pairs, the top and bottom will be the same.

We accept (and halt) when the lengths are the same and we reject (and halt) otherwise. Therefore, as we halt in any case, the problem is decidable.

*b) Show that the Post Correspondence Problem is undecidable over the binary alphabet S = {0,1}.*

We can encode every string in a finite alphabet into a binary string (like a computer using binary to encode text). As *PCP* for a random alphabet is undecidable, a random encoding in binary is also undecidable.

*c) In the *Silly Post Correspondence Problem (SPCP)*, in each pair the top string has the same length as the bottom string. Show that the *SPCP* is decidable.*

As the pairs have equal top and bottom lengths, the total length of the top word will be the same as the total length of the bottom word. That means the only way this problem is accepted, is if the top and bottom in the pair are the same.

<pre>
For each pair:
     Check if top and bottom are equal.
	 If yes, continue to next pair.
	 If no, REJECT.

If all the pairs are parsed, ACCEPT.
</pre>
