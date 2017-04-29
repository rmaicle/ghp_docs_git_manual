---
title: git-var
layout: documentation
categories: [documentation]
tags: [git]
draft: true
published: true
permalink: /doc/git/manual/ch2/sec2/git_var
group: git
---

NAME
====

git-var - Show a Git logical variable

SYNOPSIS
========

    git var ( -l | <variable> )

DESCRIPTION
===========

Prints a Git logical variable.

OPTIONS
=======

`-l`

:   Cause the logical variables to be listed. In addition, all the variables of the Git configuration file .git/config are listed as well. (However, the configuration variables listing functionality is deprecated in favor of `git config -l`.)

EXAMPLE
=======

    $ git var GIT_AUTHOR_IDENT
    Eric W. Biederman <ebiederm@lnxi.com> 1121223278 -0600

VARIABLES
=========

`GIT_AUTHOR_IDENT`

:   The author of a piece of code.

`GIT_COMMITTER_IDENT`

:   The person who put a piece of code into Git.

`GIT_EDITOR`

:   Text editor for use by Git commands. The value is meant to be interpreted by the shell when it is used. Examples: `~/bin/vi`, `$SOME_ENVIRONMENT_VARIABLE`, `"C:\Program Files\Vim\gvim.exe"
        --nofork`. The order of preference is the `$GIT_EDITOR` environment variable, then `core.editor` configuration, then `$VISUAL`, then `$EDITOR`, and then the default chosen at compile time, which is usually *vi*.

`GIT_PAGER`

:   Text viewer for use by Git commands (e.g., *less*). The value is meant to be interpreted by the shell. The order of preference is the `$GIT_PAGER` environment variable, then `core.pager` configuration, then `$PAGER`, and then the default chosen at compile time (usually *less*).

SEE ALSO
========

linkgit:git-commit-tree\[1\] linkgit:git-tag\[1\] linkgit:git-config\[1\]

GIT
===

Part of the linkgit:git\[1\] suite