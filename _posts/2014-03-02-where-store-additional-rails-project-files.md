---
layout: post
title:  "Where to store Rails project support files"
date:   2014-03-02 12:26:12
categories: rails
---

When we generate a new Rails project, it spits out at us a nice well thought-out folder structure
with a particular place for each code component should go to. App code goes into `/app` folder,
configurations into `/config`, log files int `/log` and so forth. We love it and it works great
until you come across a situation when you've got other files that you also need to
associate with the project, but your generated rails app doesn't seem to have a folder for.

<!--more-->

Example of such files (let's call them "support files" for brevity) could be:

* UX designs
* Support documentation to be distributed to the client
* Guidelines docs for the project team
* Creative source files (PSD, AI, INDD etc.)
* ... ... ...

One option would be to place all these support files into a separate from Rails app project folder
and commit them to version control (VC). While this may work for some teams on some type of projects, it
didn't really work well for us, due to the fact that we use a workflow that requires a new branch for
each new change/feature request and each such change usually requires modification to the Rails app
project code as well as modifications to some other support files. Since we use one single feature branch
for each change--keeping support files in a separate from Rails app location sooner or later
(rather sooner) leads to a situation when someone forgets to create a new branch for the support files,
creates it incorrectly, or forget to commit it. This leads to support files fall out of sync with
the main Rails app project. Bad!

What worked well for us is to bring all support files, let's call them resources, under the Rails
app umbrella. This way user story related changes gets branched and merged at the same time.

We usually add new `resources` directory and group files like this:

```
Rails_app/
----------
  |___app/
  |___bin/
  |___config
  |___... ... ...
  |___resources/
        |___/creative = all creative resources for the site (i.e. PSDs, AIs, PNGs, etc.)
        |___/docs     = all documentation related to the project
        |___/ux       = UX designs and US related stuff
```

Branch and merge everything atomically now. Nothing falls behind.

<div class="alert alert-warning">
<i class="fa fa-lightbulb-o fa-2x"></i>
If your team is rather large and
not collocated, self-organized, you may want to consider limiting access for different teams to
different folders withing the `Rails_app/` folder. So, for example, railers would have access to Rails app
code, creative designers to `resources/creative/*`, UX designers to `resources/ux/*` and so forth.
</div>

Would be interesting to hear what strategy worked for your teams. Please share in the comments below.

___
##### Today my environment was:

- Rails 4.1.0.rc1
