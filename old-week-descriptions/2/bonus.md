---
title: "This week's bonus"
description: "Automatic transcribing of audio from video"
date: 2020-01-28T00:10:51+09:00
draft: false
weight: 0
author: "Shawn Graham"
---

Historian's don't deal with video very well. That is to say, we don't have many tools for dealing with video _as video_, as a moving image, and analyzing with an eye towards the specific affordances of the medium (but see Arnold and Tilton's ['Distant Viewing'](http://distantviewing.org) project, and [their position paper](https://distantviewing.org/pdf/distant-viewing.pdf).)

But we're good with text. In this bonus exercise, you'll download a video from youtube of Dr. Martin Luther King's 'I have a dream' speech ([wikipedia](https://en.wikipedia.org/wiki/I_Have_a_Dream)), and use Google's speech-to-text engine to generate a time-stamped transcript of the speech.

NB. This works with historic audio recordings too, but of course, the better the audio quality, the better the transcript. You could even record yourself speaking and use this to make a transcript of audio notes.

### Get the code

We're going to use this [repository](https://github.com/akras14/speech-to-text) by Alex Kras; you can read his original blog post describing his project [here](https://www.alexkras.com/transcribing-audio-file-to-text-with-google-cloud-speech-api-and-python/).

1. Open terminal or anaconda powershell, and make a new python environment with `conda create -n audio python=3.7 anaconda`

2. activate your environment: `conda activate audio`

3. get the repository with: `git clone https://github.com/akras14/speech-to-text.git`

4. change directories so that you're in that repo: `cd speech-to-text`

5. install the necessary bits-and-pieces that Kras put into his requirements file: `pip install -r requirements.txt`

### Getting access to the Google API

Follow Kras's instructions to get an api key to access the speech-to-text engine:

+ [Sign up for the free tier](https://cloud.google.com/free/) and then sign in to [Google Cloud Console](https://console.cloud.google.com/)
+ Click “APIs & Services”
+ Click “Credentials”
+ Click “Create Credentials”
+ Select “Service Account Key”
+ Under “Service Account” select “New service account”
+ Name service (whatever you’d like)
+ Select Role: “Project” -> “Owner”
+ Leave “JSON” option selected
+ Click “Create”
+ Save generated API key file
+ Rename file to api-key.json

The file `api-key.json` needs to be saved inside your copy of the speech-to-text folder (eg, `\speech-to-text\api-key.json`). Note that Google sometimes changes up its interface a bit, and so the exact sequence of clicks might not be the same as described above.

### Some video utilities

1. Download and install [youtube-dl](https://youtube-dl.org/) or install it from the command line:
`pip install --upgrade youtube_dl`.
If you try the command line and get an error about not having permission to do this, try
`sudo pip install --upgrade youtube_dl`
This will require you to enter your computer username and password. Note that when you enter the password, the cursor **won't** move. This is old-school security in action...

2. Download and install ffmpeg.
On a mac - if you haven't already, get homebrew first (a package manager for downloading and installing things like this) [here](https://brew.sh/). **That website gives you a command to run in your terminal to download and install homebrew**. (But if you successfully installed Brew when you did the wget work, then you just need to do the brew install command). Then, when homebrew is installed: `brew install ffmpeg`

On a pc: download and install from [here](https://ffmpeg.zeranoe.com/builds/)

### Now let's do this!

1. There's a copy of the 'I have a dream' speech uploaded by this user at: https://www.youtube.com/watch?v=I47Y6VHc3Ms. Note the bit after ?v=. That's the id of the video we're after.
2. There are several versions of each video on youtube, to account for the different kinds of devices and resolution and so on. We'll use  youtube-dl to examine the different versions of this video, so that we can find the smallest one. At the command prompt, type `youtube-dl -F I47Y6VHc3Ms`
3. The `-F` (upper case!) tells youtube-dl to go grab all of the info and print it out for us. Each line is a different version of the video, and each line has a unique number id. We look for the line that tells us which version is smallest. Today, that's line 249 (notice that it also tells you that this is just the audio only). So now we tell youtube-dl to download _only_ that version: `youtube-dl -f 249 I47Y6VHc3Ms`. The `-f` (lower case!) flag lets us select the line number we want, and then the ID string tells youtube which video we're after.
4. Use your file explorer to rename the file that you just downloaded to something sensible; I changed mine to 'mlk.webm'.
5. Now we'll convert the file to a `.wav` file with the `ffmpeg` command: `ffmpeg -i mlk.webm mlk.wav`
6. Move the file into the `/source` folder.
7. Now we're ready to chop the file into 30 second increments. Here we go: `ffmpeg -i source/mlk.wav -f segment -segment_time 30 -c copy parts/out%09d.wav`
9. But before we pass those to google, note that files that contain dead air or nothing that is recognizable as speech can break things. Listen to the first two or three files you just created (in your file explorer, click on them). Delete the ones that are dead air.
10. ...and now pass on the remaining filesto Google for processing! Make sure you're in the correct location (eg, the `speech-to-text` folder) and type `python slow.py > results.txt`

A little progress bar will appear, and your code is passing your files one at a time up to google for processing; google is sending back to you the text! When it's done, just open up the results.txt file.

![results](/images/i-have-a-dream.png)

If you have audio from other sources you'd like to try, just follow the pattern for converting it to `.wav` format, place it in the correct place (`\source`), clean out the `\parts` subdirectory, modify the command in step 7 with your own file name, and away you'll go!
