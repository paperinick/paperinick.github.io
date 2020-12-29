---
author: Jacopo Schiavon
title: "How to be a blog posters"
date: "2020-11-01"
subtitle: "Short tutorial on how to create blog posts for paperinick."
tags: ["guide"]
ShowToc: true
TocOpen: true
weight: 2
---

_Short tutorial on how to create blog posts for paperinik._


The website is hosted in a [Github repository](https://github.com/paperinick/paperinick.github.io). To allow contribution from everyone, we will use the standard git collaborative framework based on forks and pull requests.

## Setting up the fork and the local repository

The first step, thus, is to fork the repository: from the repository page, in the top right corner of the page, press the Fork button to create a fork on your account and then clone the forked repository on your local machine. You can easily do this by opening a terminal or a Git Bash[^1] window in the location where you want the repository to be located and issuing 
```
$ git clone https://github.com/<yourname>/paperinick.github.io
```
Note that here and in the following the $ symbol is used to indicate the input of the command shell (it should not be written in the terminal window) and is opposed to the > symbol which indicate the output of the shell.

[^1]: If you don't know how to use git, a very comprehensive introduction to the former is [here](https://git-scm.com/docs/user-manual), and [here](https://git-scm.com/doc) you can also find some introductory videos. 
	Git Bash is a small program that allows Microsoft Windows user to issue the same commands that Unix users will issue from the terminal, allowing me to spell the commands only once. It is installed directly when you install git, and can be opened by right-clicking in the folder where you want to use git. Some (if not most) of the work can be done directly from github.com following the instructions on the site on how to add a file and do a commit.

When the cloning operation is done, move in the folder with the repository and configure the upstream url. You can do this by issuing:
```
$ git remote add upstream https://github.com/paperinick/paperinick.github.io
```
and then checking with
```
$ git remote -v
> origin	https://github.com/<yourname>/paperinick.github.io.git (fetch)
> origin	https://github.com/<yourname>/paperinick.github.io.git (push)
> upstream	https://github.com/mascaretti/paperinick.github.io.git (fetch)
> upstream	https://github.com/mascaretti/paperinick.github.io.git (push)
```
that everything is fine.

With this, your setup is complete.

## Adding a new post
When you want to add a new post, navigate to your local repository and create a new file in `_posts`.
You can copy and rename one of the already created posts, for instance this one, in order to follow the structure. Don't delete any of the already existing files. It is important to follow this convention for the file names:
```
yyyy-mm-dd-<post-title>.md
```
where `yyyy` is the 4 digit year, then 2 digit month and the 2 digit day.

When you are done writing your content in simple markdown sintax [^2], you can open a terminal in the folder of the repository (or a git bash window if you are on windows) and issue the following commands:
```
$ git add _posts/<yourpost>
$ git commit -m "a short message that identifies your commit"
```
As a message for the commit, you can write some info on the post, for instance "added post on the paperinik in XX/XX/2020" or something similar. You can later modify your file and commit it again following the same procedure.

[^2]: A comprehensive guide to markdown can be found [here](https://www.markdownguide.org/) where there are both a basic and extended syntax references.

If you need to add some images, you can place those in the folder `assets/images/`, possibly creating a subfolder for your post in order to keep stuff well organized. You then need to commit also the images: it should be enough to replace the previous `git add` with
```
$ git add content/posts/<yourpost>.md static/images/<yourimagefolder>/
```
and committing using the same command as before.

To write math, you can use most of standard LaTeX stuff: to introduct a display equation you should leave a blank line before and after it, while using the `$$` symbol as a delimiter:

$$x^2$$

To add an inline equation, use `$$` and `$$` delimiters but without a blank line before and after the equation, as in $$x^2$$.


## Syncing the fork with upstream
When you are done and you want to publish your post, first you should sync your local copy of the repository with the upstream one, in order to have the latest version of the website.
To do so, you should first fetch the upstream repository commits:
```
$ git fetch upstream
```
Now the upstream work should be stored in a local branch called `upstream/master`. 
We first switch back to our local master branch by issuing
```
$ git checkout master
```
and then we perform the actual merge: this will put all the stuff from the upstream repository (the common one) to your local one, adding your local changes.
```
$ git merge upstream/master
```
If this is done without problem, you can push your work to the online fork of the repository
```
$ git push origin master
```
This will ensure that the online version of your fork will be updated with your last work (and also with the work done in the common repository).

Finally, open the github page of the common repository (the one available [here](https://github.com/mascaretti/paperitivo)) and navigate to the [Pull Request](https://github.com/mascaretti/paperitivo/pulls) tab. There, you can push the "New pull request" button.

![Creating a new pull request: the compare page](/assets/images/posting-guide/compare-page.png)

A "Compare Change" page will open, from where you can check what you are trying to accomplish. Press the "compare across forks" link and select your fork. 

![Creating a new pull request: select your fork](/assets/images/posting-guide/compare-forks.png)

Review the changes to avoid any error and finally press "Create pull request" and completing the form adding a brief explanation (if needed).

Almost all this topics are described in wide detail in the GitHub Docs, for instance [How to configure a remote for a fork](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/configuring-a-remote-for-a-fork), [How to sync a fork](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/syncing-a-fork) and [How to create a pull request](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork).

