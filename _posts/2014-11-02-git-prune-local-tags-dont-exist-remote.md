---
layout: post
title:  "How to prune local git tags that don't exist on remote"
date:   2014-11-02 11:08:00
categories: git
---

As of git `1.9.4`, it appears that there is no easy way to remove local tags
that don't exist on remote (a.k.a "stale tags"). According
to `--prune` option in `git fetch` [documentaiton](http://git-scm.com/docs/git-fetch) :

<!--more-->

>  Tags are not subject to pruning if they are fetched only because of the
default tag auto-following or due to a `--tags` option. However,
if tags are fetched due to an explicit refspec
(either on the command line or in the remote configuration,
for example if the remote was cloned with the `--mirror` option),
then they are also subject to pruning..

To put it simple, if you are trying to do something like `git fetch -p -t`,
it will not work starting with git version `1.9.4`.

However, there is a simple workaround that still works in latest versions:

```bash
  $ git tag -l | xargs git tag -d # remove all local tags
  $ git fetch -t                  # fetch remote tags
```

A one liner can be written as:

```bash
  $ git tag -l | xargs git tag -d && git fetch -t
```

Alternative, you can add a new alias to your `~/.gitconfig` file to make things
shorter:

```bash
# in ~/.gitconfig
[alias]
    ... ... ... # your existing aliases
    pt = !git tag -l | xargs git tag -d && git fetch -t
```

Now, you can simply call `pt` alias to prune local stale tags:

```bash
  $ git pt
```

<div id='footer-spacing' ></div>
<p class="text-center" style="line-height:20px;">
  <a class="twitter-follow-button"
    href="https://twitter.com/demisx1"
    data-show-count="false"
    data-lang="en"
    data-size="large">Follow @demisx1
  </a> for more tips and best practices.
</p>

<div id='footer-spacing'></div>

___

Today my environment was:

- Git 1.2.1
- Mac OS X 10.10
