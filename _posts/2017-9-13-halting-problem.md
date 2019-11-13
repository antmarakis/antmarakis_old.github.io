---
layout: post
title: The Halting Problem
category: Computation Theory
---

The Halting Problem is a famous problem in computation theory, which states that we cannot build a Turing Machine that can predict whether a Turing Machine will accept on an input.

Suppose we have a machine called *Oracle*. The machine takes as input a machine *M* and a word and can predict whether *M* will accept or not the given word. This is how *Oracle* works:

* *accepts*, if *M* accepts the word
* *rejects*, if *M* doesn't accept the word

Let's say we have another machine, *D*, which takes as input a machine *M* and utilizes the *Oracle* to predict whether *M* will accept given its own encoding (usually denoted by *&#60;M&#62;*) as input. There is a little twist though. *D* outputs the *opposite* of what the *Oracle* predicts! This is how it works:

* *rejects*, if *Oracle* accepts on *M* given word *&#60;M&#62;*
* *accepts*, if *Oracle* rejects on *M* given *&#60;M&#62;*

So, if we run *D* on a machine *M*, it outputs the opposite of what the machine does.

What if we input *D* to itself? If the *Oracle* predicts *D* will accept *&#60;D&#62;*, *D* will output the opposite, which means it will reject. If the *Oracle* predicts *D* will not accept its encoding, *D* will accept.

You can see that there is a paradox, because *D* cannot reject and accept at the same time. That means no such machines as *D* and the *Oracle* can exist, therefore we cannot build a machine that predicts how a machine will behave at all times.

#### Diagonalization

An interesting way to map this paradoxical behavior out is by using Cantor's Diagonalization Technique. A node at index *(i, j)* in the table denotes whether <i>M<sub>i</sub></i> accepts or not <i>M<sub>j</sub></i>. Therefore, the nodes at *(i, i)* denote whether the machine will accept its own encoding (essentially *Oracle*'s work).

Note that since *D* is also a Turing Machine, it will also appear in the table at some point. What will that row look like? Since *D* outputs the opposite of what *Oracle* outputs, it means the row will hold values opposite to what the *(i, i)* nodes hold. For example, if the node *(e, e)* holds the value *accept*, the *e-th* node in *D*'s row will hold *reject*.

If *D*'s row is at index *d*, what then happens at node *(d, d)*? Will we see *accept* or *reject*? The answer is, we cannot know. The node doesn't have a value, so we cannot write down its opposite. It's a paradox, and thus the machine cannot exist.

Here's a visualization of the above:

<img src="https://raw.githubusercontent.com/MrDupin/mrdupin.github.io/master/images/halting_problem.png">
