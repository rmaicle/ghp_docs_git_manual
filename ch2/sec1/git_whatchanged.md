---
title: git-whatchanged
layout: documentation
categories: [documentation]
tags: [git]
draft: true
published: true
permalink: /doc/git/manual/ch2/sec1/git_whatchanged
group: git
---

NAME
====

git-whatchanged - Show logs with difference each commit introduces

SYNOPSIS
========

    git whatchanged <option>…

DESCRIPTION
===========

Shows commit logs and diff output each commit introduces.

New users are encouraged to use linkgit:git-log\[1\] instead. The `whatchanged` command is essentially the same as linkgit:git-log\[1\] but defaults to show the raw format diff output and to skip merges.

The command is kept primarily for historical reasons; fingers of many people who learned Git long before `git log` was invented by reading Linux kernel mailing list are trained to type it.

Examples
========

`git whatchanged -p v2.6.12.. include/scsi drivers/scsi`

:   Show as patches the commits since version *v2.6.12* that changed any file in the include/scsi or drivers/scsi subdirectories

`git whatchanged --since="2 weeks ago" -- gitk`

:   Show the changes during the last two weeks to the file *gitk*. The "--" is necessary to avoid confusion with the **branch** named *gitk*

GIT
===

Part of the linkgit:git\[1\] suite