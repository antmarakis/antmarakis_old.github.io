---
layout: post
title: Turing Machine Exercises
category: Computation Theory
---

*Found in Michael Sipser's "Introduction to Computation Theory", as Exercise 3.8*

---

Below are some examples of Turing Machine implementation-level algorithms to decide the following languages over the alphabet {0,1}.

a) {*w* &#124; *w* contains an equal number of 0s and 1s} <br>
b) {*w* &#124; *w* contains twice as many 0s as 1s} <br>
c) {*w* &#124; *w* does not contain twice as many 0s as 1s}

a) For input word *w*:
<pre>
1) Scan the tape and mark the first unmarked 0.
If no 0s are found go to step 3.
Otherwise, move the head back to the start of the tape.

2) Scan the tape and mark the first unmarked 1.
If no unmarked 1 exists, REJECT.
Otherwise, move the head back to the start of the tape
and go to step 1.

3) Scan the tape and if there are no unmarked 1s,
ACCEPT. Otherwise, REJECT.
</pre>

b) For input word *w*:
<pre>
1) Scan the tape and mark the first unmarked 0.
If no 0s are found go to step 3.
Otherwise, move the head back to the start of the tape.

2) Scan the tape and mark the first unmarked 1,
then continue scanning and mark the second unmarked 1.
If there are no two unmarked 1s, REJECT.
Otherwise, move the head back to the start of the tape
and go to step 1.

3) Scan the tape and if there are no unmarked 1s,
ACCEPT. Otherwise, REJECT.
</pre>

c) This is the exact opposite of b. So, it will follow the same approach as b, but the ACCEPT and REJECT conditions are reversed. For input word *w* we now have:
<pre>
1) Scan the tape and mark the first unmarked 0.
If no 0s are found go to step 3.
Otherwise, move the head back to the start of the tape.

2) Scan the tape and mark the first unmarked 1,
then continue scanning and mark the second unmarked 1.
If there are no two unmarked 1s, ACCEPT.
Otherwise, move the head back to the start of the tape
and go to step 1.

3) Scan the tape and if there are no unmarked 1s,
REJECT. Otherwise, ACCEPT.
</pre>
