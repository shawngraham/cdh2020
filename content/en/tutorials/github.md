---
title: "Github"
description: "Keeping your work under version control"
date: 2020-01-28T00:10:51+09:00
draft: false
author: "Shawn Graham"
weight: 2
---
{{< alert theme="info" >}}
If any of the instructions below are unclear, annotate this page with hypothesis while being logged into our course reading group. If you spot someone asking for help and you can offer advice, respond to the annotation. Or ping me with an email or on discord! I'm not trying to trap you; I _want_ questions!
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

6. Give it a reasonable name; in the illustration I went with 'week one' but 'demo' or 'test' or something else reasonable is fine; tick the 'initialize with a readme' box, and hit the green commit button:

![new repo settings](/images/github/github-new-repo-settings.png)

---

7. And on the final page, hit the 'dismiss' button in the 'Github Actions box'

![dismiss actions](/images/github/github-dismiss-actions.png)

Ta Da! You now have a github account, and you've created your first repository. **Going Forward** remember that you can create a new repository from the plus sign in the top right corner:

![another new repo](/images/github/new-repo.png)

**nb** Remember to tick off the 'initialize with a readme.md' file.

---

Once you've done that, you'll be at your Github user page:

![github user page](/images/github/github-signup5.png)

---

8. Add me as a collaborator to this and subsequent **private** repositories:

![manage access](/images/github/add-user-to-private-repo-1.png)

9. **Private Repo**: Then, once you've clicked on 'manage access', click on 'invite collaborator',
![add collaborator](/images/github/add-collaborator-to-private-repo-2.png)

10. **Private Repo**: Then find my username `shawngraham`, and add me:
![add shawngraham](/images/github/add-username-private-repo.png)

### Fork - make a copy - of the HIST 3814a repo

We're going to start with an act of collaboration. I have already made a repository for you called [`hist 3814a starter`](https://github.com/shawngraham/hist3814a-starter). If you open that link in a new tab - and you're logged into Github - you can click on the 'fork' button at the top right of the page to make a copy (the metaphor here is botanical - by forking the repository, you create a new branch).

- change visibility
- make private
- add me as a collaborator


### Making a new text file (and/or a folder) on Github

You can make a new text file by clicking on the 'add file' button and then the 'create new file' option; The new file screen will open and the cursor will be blinking in the box where you write the filename. When you make a new markdown text file **remember to always use .md as the file extension** in the name.

You can also create subfolders at this point; Github will understand that if you use a ``\`` in the filename box, you are indicating a folder: if you typed `subfolder-name-goes-here\my-new-file.md` it will automatically create `my-new-file.md` in the `subfolder-name-goes-here` subfolder.

<video controls>
  <source src="/data/subfolder.mp4" type="video /mpeg">
</video >

When you are making a new text file on Github, you can specify headers, links, images, bullets, blockquotes and so on by using [markdown conventions](https://guides.github.com/features/mastering-markdown/.)

The two videos below might be a bit clearer on youtube itself.

{{<youtube 2SeeKYWXbrE >}}

### Uploading a file into Github

You can add new files from your computer by dragging and dropping them into the main repository (ie, the screen on github that displays all of the files in the repository as a list). At the end of this video, I show you how to display the image in the text of the reflection.

{{<youtube muKAh_j3Ogs >}}

### Setting up a 'repository' for your work

A 'repository' is just a folder that you've shared on Github. There are two ways to do this; the easy way and the more complex way. Luck you, you have _already_ done the easy way - you selected the `initialize with a readme`, and it's already present in your browser!

{{< alert theme="info" >}}
To create new repositories, just click on the `+` button on the top right of your Github page when you're logged in. You might want to go ahead now and create repositories for the four different parts of this course. Remember to tick off the `initialize with a readme` box. (Alternatively, you could create just one repository, but make subfolders for each part of the course.)
{{< /alert >}}

But if you didn't tick the initialize box, you've embarked on the more complex way. In the screenshot below, I created a new repository but I forgot to initialize it, and now I'm looking at this page:

![non-initialized git](/images/github/github-complex.png)

If this is you, do not despair.

#### When you've forgotten to initialize a new repo:

1. If you have a PC, [click on this link to download and install git](https://git-scm.com/download/win).
2. Make a new directory on your computer **with the same name as the repo you created above**. In my example, that would be `week-two`.
3. On a PC, right-click on the folder and select 'open a command prompt here'. On a Mac, go to System Preferences, select Keyboard > Shortcuts > Services. Look for 'New Terminal at Folder' and tick the box. Open your finder; find the folder you created, right-click and select 'open Terminal here'.
4. Do you see where, in your browser at github, it says `...or create a new repository on the command line`? Type in each line exactly as it is there (beginning with `echo`), hitting enter at the end of each line, in order. **If the command works, you'll just be presented with the next prompt.** The computer only ever responds when there is output to print - which often means only when there is an error message to report.

Ta da! You can now go to `github.com\<your-user-name>\your-repo` and you'll see it all there tickety boo.


### Going further

Github can also be used to run an entire website, generated from your text files. In fact, that's how I built this course website! The following three links are from my HIST4806a 2020 seminar on digital history and museums; they will walk you through how to use your Github account to serve up a professional scholarly website. You can use this scholarly website to host your reflections and course work, if you want (some of the minor details might have changed, but the general flow is still the same; this kind of thing always happens with digital work). Follow these three pages in order:

1. [Building Your Own Scholarly Website](https://shawngraham.github.io/dhmuse/building-your-online-presence/)

2. [Customizing Your Scholarly Website](https://shawngraham.github.io/dhmuse/customizing-your-site/)

3. [Updating Your Scholarly Website](https://shawngraham.github.io/dhmuse/updating-your-devlogs/)
