---
title: git-quiltimport
layout: documentation
categories: [documentation]
tags: [git]
draft: true
published: true
permalink: /doc/git/manual/ch2/sec1/git_quiltimport
group: git
---

NAME
====

git-quiltimport - Applies a quilt patchset onto the current branch

SYNOPSIS
========

    git quiltimport [--dry-run | -n] [--author <author>] [--patches <dir>]
                    [--series <file>]

DESCRIPTION
===========

Applies a quilt patchset onto the current Git branch, preserving the patch boundaries, patch order, and patch descriptions present in the quilt patchset.

For each patch the code attempts to extract the author from the patch description. If that fails it falls back to the author specified with --author. If the --author flag was not given the patch description is displayed and the user is asked to interactively enter the author of the patch.

If a subject is not found in the patch description the patch name is preserved as the 1 line subject in the Git description.

OPTIONS
=======

`-n`; `--dry-run`

:   Walk through the patches in the series and warn if we cannot find all of the necessary information to commit a patch. At the time of this writing only missing author information is warned about.

`--author Author Name <Author Email>`

:   The author name and email address to use when no author information can be found in the patch description.

`--patches <dir>`

:   The directory to find the quilt patches.

    The default for the patch directory is patches or the value of the `$QUILT_PATCHES` environment variable.

`--series <file>`

:   The quilt series file.

    The default for the series file is &lt;patches&gt;/series or the value of the `$QUILT_SERIES` environment variable.

GIT
===

Part of the linkgit:git\[1\] suite
