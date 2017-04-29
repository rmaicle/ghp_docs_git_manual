---
title: git-prune-packed
layout: documentation
categories: [documentation]
tags: [git]
draft: true
published: true
permalink: /doc/git/manual/ch2/sec2/git_prune_packed
group: git
---

NAME
====

git-prune-packed - Remove extra objects that are already in pack files

SYNOPSIS
========

    git prune-packed [-n|--dry-run] [-q|--quiet]

DESCRIPTION
===========

This program searches the `$GIT_OBJECT_DIRECTORY` for all objects that currently exist in a pack file as well as the independent object directories.

All such extra objects are removed.

A pack is a collection of objects, individually compressed, with delta compression applied, stored in a single file, with an associated index file.

Packs are used to reduce the load on mirror systems, backup engines, disk storage, etc.

OPTIONS
=======

`-n`; `--dry-run`

:   Don’t actually remove any objects, only show those that would have been removed.

`-q`; `--quiet`

:   Squelch the progress indicator.

SEE ALSO
========

linkgit:git-pack-objects\[1\] linkgit:git-repack\[1\]

GIT
===

Part of the linkgit:git\[1\] suite