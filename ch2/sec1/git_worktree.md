---
title: git-worktree
layout: documentation
categories: [documentation]
tags: [git]
draft: true
published: true
permalink: /doc/git/manual/ch2/sec1/git_worktree
group: git
---

NAME
====

git-worktree - Manage multiple working trees

SYNOPSIS
========

    git worktree add [-f] [--detach] [--checkout] [-b <new-branch>] <path> [<branch>]
    git worktree list [--porcelain]
    git worktree lock [--reason <string>] <worktree>
    git worktree prune [-n] [-v] [--expire <expire>]
    git worktree unlock <worktree>

DESCRIPTION
===========

Manage multiple working trees attached to the same repository.

A git repository can support multiple working trees, allowing you to check out more than one branch at a time. With `git worktree add` a new working tree is associated with the repository. This new working tree is called a "linked working tree" as opposed to the "main working tree" prepared by "git init" or "git clone". A repository has one main working tree (if it’s not a bare repository) and zero or more linked working trees.

When you are done with a linked working tree you can simply delete it. The working tree’s administrative files in the repository (see "DETAILS" below) will eventually be removed automatically (see `gc.worktreePruneExpire` in linkgit:git-config\[1\]), or you can run `git worktree prune` in the main or any linked working tree to clean up any stale administrative files.

If you move a linked working tree, you need to manually update the administrative files so that they do not get pruned automatically. See section "DETAILS" for more information.

If a linked working tree is stored on a portable device or network share which is not always mounted, you can prevent its administrative files from being pruned by issuing the `git worktree lock` command, optionally specifying `--reason` to explain why the working tree is locked.

COMMANDS
========

`add <path> [<branch>]`

:   Create `<path>` and checkout `<branch>` into it. The new working directory is linked to the current repository, sharing everything except working directory specific files such as HEAD, index, etc. `-` may also be specified as `<branch>`; it is synonymous with `@{-1}`.

    If `<branch>` is omitted and neither `-b` nor `-B` nor `--detach` used, then, as a convenience, a new branch based at HEAD is created automatically, as if `-b $(basename <path>)` was specified.

`list`

:   List details of each worktree. The main worktree is listed first, followed by each of the linked worktrees. The output details include if the worktree is bare, the revision currently checked out, and the branch currently checked out (or *detached HEAD* if none).

`lock`

:   If a working tree is on a portable device or network share which is not always mounted, lock it to prevent its administrative files from being pruned automatically. This also prevents it from being moved or deleted. Optionally, specify a reason for the lock with `--reason`.

`prune`

:   Prune working tree information in $GIT\_DIR/worktrees.

`unlock`

:   Unlock a working tree, allowing it to be pruned, moved or deleted.

OPTIONS
=======

`-f`; `--force`

:   By default, `add` refuses to create a new working tree when `<branch>` is already checked out by another working tree. This option overrides that safeguard.

`-b <new-branch>`; `-B <new-branch>`

:   With `add`, create a new branch named `<new-branch>` starting at `<branch>`, and check out `<new-branch>` into the new working tree. If `<branch>` is omitted, it defaults to HEAD. By default, `-b` refuses to create a new branch if it already exists. `-B` overrides this safeguard, resetting `<new-branch>` to `<branch>`.

`--detach`

:   With `add`, detach HEAD in the new working tree. See "DETACHED HEAD" in linkgit:git-checkout\[1\].

`--[no-]checkout`

:   By default, `add` checks out `<branch>`, however, `--no-checkout` can be used to suppress checkout in order to make customizations, such as configuring sparse-checkout. See "Sparse checkout" in linkgit:git-read-tree\[1\].

`-n`; `--dry-run`

:   With `prune`, do not remove anything; just report what it would remove.

`--porcelain`

:   With `list`, output in an easy-to-parse format for scripts. This format will remain stable across Git versions and regardless of user configuration. See below for details.

`-v`; `--verbose`

:   With `prune`, report all removals.

`--expire <time>`

:   With `prune`, only expire unused working trees older than &lt;time&gt;.

`--reason <string>`

:   With `lock`, an explanation why the working tree is locked.

`<worktree>`

:   Working trees can be identified by path, either relative or absolute.

    If the last path components in the working tree’s path is unique among working trees, it can be used to identify worktrees. For example if you only have two working trees, at "/abc/def/ghi" and "/abc/def/ggg", then "ghi" or "def/ghi" is enough to point to the former working tree.

DETAILS
=======

Each linked working tree has a private sub-directory in the repository’s $GIT\_DIR/worktrees directory. The private sub-directory’s name is usually the base name of the linked working tree’s path, possibly appended with a number to make it unique. For example, when `$GIT_DIR=/path/main/.git` the command `git worktree add /path/other/test-next next` creates the linked working tree in `/path/other/test-next` and also creates a `$GIT_DIR/worktrees/test-next` directory (or `$GIT_DIR/worktrees/test-next1` if `test-next` is already taken).

Within a linked working tree, $GIT\_DIR is set to point to this private directory (e.g. `/path/main/.git/worktrees/test-next` in the example) and $GIT\_COMMON\_DIR is set to point back to the main working tree’s $GIT\_DIR (e.g. `/path/main/.git`). These settings are made in a `.git` file located at the top directory of the linked working tree.

Path resolution via `git rev-parse --git-path` uses either $GIT\_DIR or $GIT\_COMMON\_DIR depending on the path. For example, in the linked working tree `git rev-parse --git-path HEAD` returns `/path/main/.git/worktrees/test-next/HEAD` (not `/path/other/test-next/.git/HEAD` or `/path/main/.git/HEAD`) while `git
rev-parse --git-path refs/heads/master` uses $GIT\_COMMON\_DIR and returns `/path/main/.git/refs/heads/master`, since refs are shared across all working trees.

See linkgit:gitrepository-layout\[5\] for more information. The rule of thumb is do not make any assumption about whether a path belongs to $GIT\_DIR or $GIT\_COMMON\_DIR when you need to directly access something inside $GIT\_DIR. Use `git rev-parse --git-path` to get the final path.

If you move a linked working tree, you need to update the *gitdir* file in the entry’s directory. For example, if a linked working tree is moved to `/newpath/test-next` and its `.git` file points to `/path/main/.git/worktrees/test-next`, then update `/path/main/.git/worktrees/test-next/gitdir` to reference `/newpath/test-next` instead.

To prevent a $GIT\_DIR/worktrees entry from being pruned (which can be useful in some situations, such as when the entry’s working tree is stored on a portable device), use the `git worktree lock` command, which adds a file named *locked* to the entry’s directory. The file contains the reason in plain text. For example, if a linked working tree’s `.git` file points to `/path/main/.git/worktrees/test-next` then a file named `/path/main/.git/worktrees/test-next/locked` will prevent the `test-next` entry from being pruned. See linkgit:gitrepository-layout\[5\] for details.

LIST OUTPUT FORMAT
==================

The worktree list command has two output formats. The default format shows the details on a single line with columns. For example:

    S git worktree list
    /path/to/bare-source            (bare)
    /path/to/linked-worktree        abcd1234 [master]
    /path/to/other-linked-worktree  1234abc  (detached HEAD)

Porcelain Format
----------------

The porcelain format has a line per attribute. Attributes are listed with a label and value separated by a single space. Boolean attributes (like *bare* and *detached*) are listed as a label only, and are only present if and only if the value is true. An empty line indicates the end of a worktree. For example:

    S git worktree list --porcelain
    worktree /path/to/bare-source
    bare

    worktree /path/to/linked-worktree
    HEAD abcd1234abcd1234abcd1234abcd1234abcd1234
    branch refs/heads/master

    worktree /path/to/other-linked-worktree
    HEAD 1234abc1234abc1234abc1234abc1234abc1234a
    detached

EXAMPLES
========

You are in the middle of a refactoring session and your boss comes in and demands that you fix something immediately. You might typically use linkgit:git-stash\[1\] to store your changes away temporarily, however, your working tree is in such a state of disarray (with new, moved, and removed files, and other bits and pieces strewn around) that you don’t want to risk disturbing any of it. Instead, you create a temporary linked working tree to make the emergency fix, remove it when done, and then resume your earlier refactoring session.

    $ git worktree add -b emergency-fix ../temp master
    $ pushd ../temp
    # ... hack hack hack ...
    $ git commit -a -m 'emergency fix for boss'
    $ popd
    $ rm -rf ../temp
    $ git worktree prune

BUGS
====

Multiple checkout in general is still experimental, and the support for submodules is incomplete. It is NOT recommended to make multiple checkouts of a superproject.

git-worktree could provide more automation for tasks currently performed manually, such as:

-   `remove` to remove a linked working tree and its administrative files (and warn if the working tree is dirty)

-   `mv` to move or rename a working tree and update its administrative files

GIT
===

Part of the linkgit:git\[1\] suite
