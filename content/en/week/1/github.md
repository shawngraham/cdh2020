---
title: "Setting up your Github"
description: "instructions"
date: 2020-01-28T00:10:51+09:00
draft: false
weight: -2
author: "Shawn Graham"
---
{{< alert theme="info" >}}
If any of the instructions below are unclear, annotate this page with hypothesis while being logged into our course reading group. If you spot someone asking for help and you can offer advice, respond to the annotation.
{{< /alert >}}

### Introduction

Github is a code sharing website often used by digital historians. 'Git' is a program that takes snapshots of the current state of a folder, and stores them in sequence, allowing you to revert your changes to an earlier state. It also allows you to create **branches**, or duplicates of your folder, so you can experiment. If you like the results of your work, you can **merge** that branch back into your original.

**Github therefore is a hub for sharing these git snapshots.**

A branch (a copy, a duplicate) of your work uploaded to Github could therefore be copied to say my account (a **fork**); then I might download to my machine to work on it (I've **pulled** it). Once I'm happy with my changes, I **commit** them (save them to the sequence of changes that Git tracks), and then I could **push** those changes to the **fork** in my account. I would then send you a **pull** notice, asking you to pull my changes back to your account; you could then decide whether or not to **merge**.

{{< alert theme="info" >}}
For our purposes, you will use Github mostly as a place to put your reflections or other pieces of work.
{{< /alert >}}

I might sometimes fork your work, pull it down onto my machine, make changes that I commit, push it back online, and ask you to pull the changes back. But that won't happen very often. It takes a while to get comfortable with this.

### Get your account

1. Got to [github.com](http://github.com) and sign up for an account. You don't have to use your real name. (Protip: You might want to set up an email just for signing up for things.)

![github login screen](/images/github/github-signup1.png)

---

2. Verify your account.

![github account verification](/images/github/github-signup2.png)

---

3. Select the free tier (nb: I will never require you to pay for any reading, or any software. If something wants you to pay, **stop** and ask for help).

![github tier selection](/images/github/github-signup3.png)

---

4. Skip telling Github anything about yourself.

![github survey](/images/github/github-signup4.png)

---

5. Do the verification email thing. Once you hit the verification link in your email, you'll be brought back to Github to make a new repository:

![new repo](/images/github/github-new-repo.png)

---

6. Give it a reasonable name; tick the 'initialize with a readme' box, and hit the green commit button:

![new repo settings](/images/github/github-new-repo-settings.png)

---

7. And on the final page, hit the 'dismiss' button in the 'Github Actions box'

![dismiss actions](/images/github/github-dismiss-actions.png)

Ta Da! You now have a github account, and you've created your first repository. **Going Forward** create a new repository for each week's work/reflection. You can create a new repository from the plus sign in the top right corner:

![another new repo](/images/github/new-repo.png)

**nb** Remember to tick off the 'initialize with a readme.md' file.

---

Once you've done that, you'll be at your Github user page:

![github user page](/images/github/github-signup5.png)

---

### Setting up a 'repository' for your work

A 'repository' is just a folder that you've shared on Github. There are two ways to do this; the easy way and the more complex way. Luck you, you have _already_ done the easy way - you selected the initialize with a readme, and it's already present in your browser.

But if you didn't tick that box off, you've embarked on the more complex way. In the screenshot below, I created a new repository but did not initialize it.

![non-initialized git](/images/github/github-complex.png)

If this is you, do not despair.

1. If you have a PC, [click on this link to download and install git](https://git-scm.com/download/win).
2. make a new directory on your computer **with the same name as the repo you created above**. In my example, that would be `week-two`.
3. On a PC, right-click on the folder and select 'open a command prompt here'. On a Mac, go to System Preferences, select Keyboard > Shortcuts > Services. Look for 'New Terminal at Folder' and tick the box. Open your finder; find the folder you created, right-click and select 'open Terminal here'.
4. Do you see where, in your browser at github, it says '...or create a new repository on the command line'? Type in each line exactly as it is there, in order.

Ta da! You can now go to `github.com\<your-user-name>\your-repo` and you'll see it all there tickety boo.

### Making a new text file on Github

You can make a new text file by clicking on the 'create new file' button; **remember to always use .md as the file extension**. You can specify headers, links, images, bullets, blockquotes and so on by using [markdown conventions](https://guides.github.com/features/mastering-markdown/.)

The two videos below might be a bit clearer on youtube itself.

{{<youtube 2SeeKYWXbrE >}}

### Uploading a file into Github

You can add new files from your computer by dragging and dropping them into the main repository. At the end of this video, I show you how to display the image in the text of the reflection.

{{<youtube muKAh_j3Ogs >}}

### Going further

Github can also be used to run an entire website, generated from your text files. In fact, that's how I built this course website! The following three links are from my HIST4806a 2020 seminar on digital history and museums; they will walk you through how to use your Github account to serve up a professional scholarly website; you can use this scholarly website to host your reflections and course work, if you want.

1. [Building Your Own Scholarly Website](https://shawngraham.github.io/dhmuse/building-your-online-presence/)

2. [Customizing Your Scholarly Website](https://shawngraham.github.io/dhmuse/customizing-your-site/)

3. [Updating Your Scholarly Website](https://shawngraham.github.io/dhmuse/updating-your-devlogs/)
