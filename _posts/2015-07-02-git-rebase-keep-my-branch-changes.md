---
layout: post
title:  "How to rebase against another branch overriding conflicts with your own branch changes"
date:   2015-07-02 9:34:00
categories: git rebase
---

Quite often I find myself in a situation when I need to rebase my feature branch containing 
the latest code against the `master`, but running `git rebase master` generates a bunch of 
conflicts that I am expected to fix manually, though I know that my feature branch 
has the latest and greatest and I simply want the files in my feature branch overwrite 
the corresponding files in `master` each time such conflict arises. 
This usually happens when I am the only committer on the project.

<!--more-->

Starting with git version 1.7.3 it became possible to pass a strategy option to `git rebase` command. 
Thus, if I want to rebase to `master` and tell git to use my feature branch as the latest source when 
conflicts arise, I need to do the following:

```bash
$ git checkout [my-feature-branch]
$ git rebase -Xtheirs master
```

Vice versa, if you want to rebase onto another branch and use that branch as the latest source when
resolving conflicts use `-Xours` option instead:

```bash
$ git checkout [my-feature-branch]
$ git rebase -Xours master
```

The use of `-Xtheirs` and `-Xours` appear to be somewhat counterintuitive, so think of it as telling
git which branch files to override when resolving rebase conflicts, i.e. `-Xtheirs` will keep 
your branch changes by overwriting files in the branch you are rebasing to and `-Xours` will 
overwrite your own branch with the original files in the branch your are committing to.

Similar options exist in `git merge` command as well.

Today my environment was:

- git 2.4.2
- Mac OS X 10.10.3
