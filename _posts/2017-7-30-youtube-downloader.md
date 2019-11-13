---
layout: post
title: Youtube Sample Video Downloader
category: Tutorials
---

In this tutorial I will show you how to download sample videos and their tags from Youtube quickly and easily with Python 3. So, if you need a dataset of videos and information on them for your Machine Learning adventures, you came to the right place.

## Requirements

You will need the following Python 3 libraries:

* Numpy
* Pafy
* Tensorflow (CPU-only version adequate)
* Youtube-dl

To install these:

* `pip install numpy`
* `pip install pafy`
* [Instructions](https://www.tensorflow.org/install/) (usually `pip install tensorflow` will work)
* `pip install youtube-dl`

Depending on your setup, you might need to run the above as an admin, or use `pip3` instead of `pip`.

## Dataset

The dataset we will download is [Youtube-8M](https://research.google.com/youtube8m/). It is a huge dataset by Google, containing videos and some hand-picked tags. The dataset is split into two categories: training and testing. We will be focusing on the training dataset, which includes tags, but the process is very similar for the testing dataset too, if you wish to download it.

## Step 1: Download Tensorflow Records

Youtube-8M provides a neat tool to download Tensorflow records, which contain information about the videos (such as tags, id etc.). You can find the Python script [here](http://data.yt8m.org/download.py). Also, you can find instructions on its usage [here](https://research.google.com/youtube8m/download.html), under the "Video-level features dataset" section. Basically, you run the following command in the directory you want the records stored:

`curl data.yt8m.org/download.py | shard=1,100 partition=1/video_level/train mirror=us python`

Where `us` you can put `eu` or `asia`, depending on your continent. Also, with the `shard` keyword you can specify how many records you want downloaded. `1, 100` means the 1/100th of the dataset.

The above will download many tf_records files (for example, `train0_.tfrecord`).

## Step 2: Extract Info

After that, you can extract information from these records and store them in a file using [videos.py](https://github.com/MrDupin/Youtube-Sample-Video-Downloader/blob/master/videos.py). The script takes as argument a record file name (eg. `train0_.tfrecord` for the previously mentioned record), downloads the video and stores its id, tags, title and description in a file (`save.txt`).

First, using Tensorflow it parses the record into video examples. From there we get the video id and the tags of the video. Then, using Pafy, we get the length. If it is no more than 150 seconds we continue (you can change the seconds limit to whatever you want, or even remove it). After that, we download the video, its description and its title using youtube-dl. The `ydl_opts = {'format':'135+140'}` means we download both video and audio.

Finally, we store the video id, title, tags and description in a text file .

---

You can find the code [on my Github](https://github.com/MrDupin/Youtube-Sample-Video-Downloader). Thanks for reading!
