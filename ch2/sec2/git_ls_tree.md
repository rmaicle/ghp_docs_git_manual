---
title: git-ls-tree
layout: documentation
categories: [documentation]
tags: [git]
draft: true
published: true
permalink: /doc/git/manual/ch2/sec2/git_ls_tree
group: git
---

NAME
====

git-ls-tree - List the contents of a tree object

SYNOPSIS
========

    git ls-tree [-d] [-r] [-t] [-l] [-z]
                [--name-only] [--name-status] [--full-name] [--full-tree] [--abbrev[=<n>]]
                <tree-ish> [<path>…]

DESCRIPTION
===========

Lists the contents of a given tree object, like what "/bin/ls -a" does in the current working directory. Note that:

-   the behaviour is slightly different from that of "/bin/ls" in that the *&lt;path&gt;* denotes just a list of patterns to match, e.g. so specifying directory name (without `-r`) will behave differently, and order of the arguments does not matter.

-   the behaviour is similar to that of "/bin/ls" in that the *&lt;path&gt;* is taken as relative to the current working directory. E.g. when you are in a directory *sub* that has a directory *dir*, you can run *git ls-tree -r HEAD dir* to list the contents of the tree (that is *sub/dir* in `HEAD`). You don’t want to give a tree that is not at the root level (e.g. `git ls-tree -r HEAD:sub dir`) in this case, as that would result in asking for *sub/sub/dir* in the `HEAD` commit. However, the current working directory can be ignored by passing --full-tree option.

OPTIONS
=======

`<tree-ish>`

:   Id of a tree-ish.

`-d`

:   Show only the named tree entry itself, not its children.

`-r`

:   Recurse into sub-trees.

`-t`

:   Show tree entries even when going to recurse them. Has no effect if `-r` was not passed. `-d` implies `-t`.

`-l`; `--long`

:   Show object size of blob (file) entries.

`-z`

:   \\0 line termination on output.

`--name-only`; `--name-status`

:   List only filenames (instead of the "long" output), one per line.

`--abbrev[=<n>]`

:   Instead of showing the full 40-byte hexadecimal object lines, show only a partial prefix. Non default number of digits can be specified with --abbrev=&lt;n&gt;.

`--full-name`

:   Instead of showing the path names relative to the current working directory, show the full path names.

`--full-tree`

:   Do not limit the listing to the current working directory. Implies --full-name.

`[<path>…]`

:   When paths are given, show them (note that this isn’t really raw pathnames, but rather a list of patterns to match). Otherwise implicitly uses the root level of the tree as the sole path argument.

Output Format
=============

    <mode> SP <type> SP <object> TAB <file>

Unless the `-z` option is used, TAB, LF, and backslash characters in pathnames are represented as `\t`, `\n`, and `\\`, respectively. This output format is compatible with what `--index-info --stdin` of *git update-index* expects.

When the `-l` option is used, format changes to

    <mode> SP <type> SP <object> SP <object size> TAB <file>

Object size identified by &lt;object&gt; is given in bytes, and right-justified with minimum width of 7 characters. Object size is given only for blobs (file) entries; for other entries `-` character is used in place of size.

GIT
===

Part of the linkgit:git\[1\] suite