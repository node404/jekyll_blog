---
layout: post
title: "git sync with a fork"
description: ""
category: git
tags: [git, github]
---
{% include JB/setup %}

1. Open Terminal (for Mac users) or the command prompt (for Windows and Linux users).

2. Change the current working directory to your local project.

3. Fetch the branches and their respective commits from the upstream repository. Commits to ```master``` will be stored in a local branch, ```upstream/master.```

	$ git fetch upstream

4. Check out your fork's local ```master``` branch.

	$ git checkout master

5. Merge the changes from upstream/master into your local master branch. This brings your fork's master branch into sync with the upstream repository, without losing your local changes.


	$ git merge upstream/master

6. If your local branch didn't have any unique commits, Git will instead perform a "fast-forward":


> Tip: Syncing your fork only updates your local copy of the repository. To update your fork on GitHub, you must push your changes.

---

[Syncing a fork - User Documentation](https://help.github.com/articles/syncing-a-fork/)
