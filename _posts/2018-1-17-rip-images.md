---
layout: post
title: Download Images From Reddit
category: Tutorials
---

In this tutorial I will show you how you can easily download images from Reddit (or other websites), using `RipMe`.

### Installation

The only prerequisite to installing `RipMe` is Java. After you have downloaded and installed Java, head over to the [RipMe releases page](https://github.com/ripmeapp/ripme/releases) and download the latest version (you don't need the source, just `ripme.jar`).

### RipMe

After you have downloaded `ripme.jar`, move to its directory and run the following command:

`java -jar ripme.jar`

A window will pop up. On the top of the page, paste the link to the page you want the images ripped from. Let's say I want to download the top images of [Wes Anderson-like pics](https://www.reddit.com/r/AccidentalWesAnderson/top/). I can paste the url in and I run the program. The images should be downloaded to a folder in the directory I specified (by default on a 'rips' folder in the same directory as `ripme.jar`).

### Renaming Images

Next we want to rename the files to something more readable. The following command line renames the files of a directory to padded numbers (up to three digits):

```
ls | cat -n | while read n f; do mv "$f" `printf "im_%03d.jpg" $n`; done
```

Move to the directory with the files and execute the above command in the terminal, and your images will automatically be renamed in the form `im_xxx.jpg`, where `xxx` is a zero-padded number.

### Conclusion

And that is it! We have now collected images to use as we wish, be it in a Machine Learning project or anything else.
