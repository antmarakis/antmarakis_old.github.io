---
layout: post
title: How to Update Git Submodules
category: Tutorials
---

This is a tutorial on updating [Github submodules](https://github.com/blog/2104-working-with-submodules) the quick and easy way. This might be a bit hacky, but if you are like me and you just want to know how to update submodules without learning the ins and outs of `Git`, this is for you.

First things first, a submodule is a file in a Github repository that links to another repository. An example is [`aima-python`](https://github.com/aimacode/aima-python), which links to [`aima-data`](https://github.com/aimacode/aima-data/tree/master). This is useful in organizing a project. You simply clone a repository and request that `git` downloads the contents of the other repository.

Notice how `aima-python` does not link to the master branch of `aima-data` though, instead linking to a particular branch created the moment the submodule is added. That means any changes to `aima-data` will be lost to the Python repository, since it will only download from the particular branch. So, once in a while you have to update the submodule.

I have found that this is easier said than done, and a Googling section doesn't give much information. You have to be pretty knowledgable in `git` to do it, which seems like an overkill for such a simple task. Indeed, I have found a way to hack something together.

### Basic Outline

You have to do the following things in this order:

1. Clone the repository (it's better to start clean, but it's not necessary)
1. Delete the current submodule
1. Add the new submodule
1. Push the commit

## In More Detail

Clone your repository with the following:

`git clone REPOSITORY_URL`

Move into the repository's directory:

`cd REPOSITORY_NAME`

Now for the deletion:

<pre>
git submodule deinit SUBMODULE_NAME
git rm SUBMODULE_NAME
</pre>

To add the updated submodule:

`git submodule add SUBMODULE_URL SUBMODULE_NAME`

Finally, commit and push. Here I push to the origin of the cloned repository, but you can do whatever you want. For example, this works for me:

<pre>
git commit
git push origin
</pre>
