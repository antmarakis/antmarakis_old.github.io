---
layout: post
title: Wait for Coroutine - A Bit of Info
category: Bits of Info
---

Let's see how we can wait for a coroutine to finish before going on with our business.

Say you have a function like this:

<pre>
void SomeFunction(){
    //do stuff before SomeCoroutine starts...

    StartCoroutine(SomeCoroutine());

    //do stuff after SomeCoroutine finishes...
}

IEnumerator SomeCoroutine(){
    //do something...
}
</pre>

One quick solution is to move the stuff you want to do after the coroutine finishes from SomeFunction to SomeCoroutine.

There is one more elegant and clean solution.

Essentially what we want to do is wait for the coroutine SomeCoroutine to finish. A function cannot do that. A coroutine though can. So, we will use another coroutine to solve this coroutine problem! I know it sounds more complicated, but bear with me. This is the code:

<pre>
IEnumerator SomeFunctionTurnedCoroutine(){
    //do stuff before SomeCoroutine starts...

    yield return StartCoroutine(SomeCoroutine());

    //do stuff after SomeCoroutine finishes...
}

IEnumerator SomeCoroutine(){
    //do something...
}
</pre>

And voila, the problem is solved. We turn SomeFunction into a coroutine and we wait for SomeCoroutine to finish (that's what the yield command is basically for, "waiting"). When it does, we continue with the execution of the function as normal.
