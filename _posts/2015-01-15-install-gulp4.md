---
layout: post
title:  "How to install Gulp 4 before it's officially released"
date:   2015-01-15 9:34:00
categories: gulp4
---

##

___

With Gulp 4 approaching its release date many would like to intall it now
and take advantage of the cool goodies the new v4 comes with.
Can't blame them&mdash;we've switched to Gulp 4 a few weeks ago and couldn't be happier.

<!--more-->

Please note, this installation assumes replacement of previous version of Gulp
with the new Gulp 4 version. It's possible to keep them both, but I haven't
tested that yet. For complete replacement, follow these instructions
until v4 is officially released on bower:


```bash

# uninstall previous Gulp installation, if any
$ npm uninstall gulp -g
$ cd [your_project_root]
$ npm uninstall gulp

# install Gulp 4 CLI tools globally from 4.0 GitHub branch
$ npm install gulpjs/gulp-cli#4.0 -g

# install Gulp 4 into your project
$ npm install gulpjs/gulp.git#4.0 --save-dev
```

Your new v4 version should be ready to use now. Here is a sample
[v4 gulpfile.js](https://gist.github.com/demisx/beef93591edc1521330a) to get you started.
Some samples are also available on the (project's 4.0 branch)
[https://github.com/gulpjs/gulp/tree/4.0].

The Gulp 4 documentation effort is on the way.
(Watch and contribute)[https://github.com/gulpjs/gulp/issues/803]
Just be careful parsing existing documentation. Some of it is still v3
and hasn't been updated.

Today my environment was:

- Gulp 4.0.0-alpha.1
- npm 2.0.0-alpha-5
- Mac OS X 10.10.1
