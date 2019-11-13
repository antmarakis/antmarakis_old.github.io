---
layout: post
title: Chomsky Hierarchy
category: Computation Theory
---

The Chomsky Hierarchy is a hiearchy of classes of grammars. It is useful in showing what language a type of grammar can recognise. More powerful grammars contain less powerful ones, and thus can also recognise language recognised by the "inferior" grammars.

<img src="http://i.imgur.com/qyyuq4K.png">

From most powerful to least powerful:

Recursively Enumerable (Type-0):

* Makes use of unrestricted rules. Both sides of each rule can have any number of terminal and non-terminal symbols.

* As expressive as Turing Machines.

* `A B C -> D E`

Context-Sensitive Grammar (Type-1):

* Right side of each rule must have at least as many symbols as the left side.

* As expressive as Linearly-Bounded Turing Machines.

* Can represent languages such as \\( a^nb^nc^n \\).

* `A X C -> A Y C`

Context-Free Grammar (Type-2):

* The left side consists of a single non-terminal symbol.

* Equivalent to a pushdown (stack) automaton.

* Can represent languages such as \\( a^nb^n \\).

Regular (Type-3):

* A non-terminal symbol on the left side, and a terminal (optionally followed by a non-terminal) symbol on the right hand side.

* Equivalent to finite-state automata.

* Can represent languages such as \\( a^\*b^\* \\).
