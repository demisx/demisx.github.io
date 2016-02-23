---
layout: post
title:  "How to rebase against another branch overriding conflicts with your own branch changes"
date:   2015-07-02 9:34:00
categories: git rebase
---

Quite often I find myself in a situation when I need to rebase my local feature branch containing 
the latest code against the `master`, but running `git rebase master` generates a bunch of 
conflicts that I am expected to fix manually, though I know that my local feature branch 
has the latest and greatest and I simply want the changes in my feature branch overwrite 
the corresponding files in `master` each time such conflict arises. 
This usually happens when I am the only committer on the project.

<!--more-->

Starting with git version 1.7.3 it became possible to pass a strategy option to `git rebase` command. 

The use of `-Xtheirs` and `-Xours` appear to be somewhat counterintuitive, so think of it as telling
git which branch code to favor when resolving rebase conflicts. For example, when doing: 

```sh
# see current branch
$ git branch
---
* branch-a
...
# rebase preferring current branch changes merge during conflicts
$ git rebase -Xtheirs branch-b
```

`-Xtheirs` will favor your current `branch-a` code when overwriting merge conflicts, 
and vice versa `-Xours` will  overwrite merge conflicts with with the code in `branch-b`.

Similar options exist in `git merge` command as well, but the meaning of `-Xtheirs` and `-Xours` is
reversed due to the differences on how `git rebase` and `git merge` operate and what they consider
`ours` vs. `theirs`:

```sh
# assuming branch-a is our current version
$ git rebase -Xtheirs branch-b # <- ours: branch-b, theirs: branch-a
$ git merge -Xtheirs branch-b  # <- ours: branch-a, theirs: branch-b
```

Thus, if you are merging changes from `origin/master` and would like 
git to favor your current branch code during merge conflicts, you'd need to do this:

```sh
$ git merge -Xours origin/master
```

Today my environment was:

- git 2.4.2
- Mac OS X 10.10.3
