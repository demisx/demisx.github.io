---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;3)"
date:   2014-12-12 10:39:00
categories: angularjs atom component-feature-based-organization
---

## Level 2 organization

Now we are ready to start reviewing organization structure at the second level.
If you've missed my previous posts,
recommend reading them first before continuing further:

* [AngularAtom Component-based organization - Introduction]({% post_url 2014-12-02-angular-1-component-organization-1 %}) (Part 1)
* [AngularAtom Component-based organization - Level 1 Organization]({% post_url 2014-12-11-angular-1-component-organization-2 %}) (Part 2)

### `app/` folder

In this post we'll examine organization of the `app/` folder.
Since the `app/` folder contains user generated application code, we divide it
like this:

<!--more-->

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l2-app.sh %}

* `components/`&mdash;container for all user written application components ([more on components...]({% post_url 2014-12-13-angular-1-component-organization-4 %}))
* `layouts/`&mdash;container for the layout definition partial files ([more on layouts...]({% post_url 2014-12-13-angular-1-component-organization-4 %}))
* `states/`&mdash;container for all application UI states ([more on states...]({% post_url 2014-12-13-angular-1-component-organization-4 %}))
* `app.js`&mdash;defines main application module and lists dependencies on components and states modules
* `index.html`&mdash;this is the application main entry point. This is where
`ng-app` directive and top level `ui-view` are placed. The latter creates a placeholder inside the `<body>...</body>` block that abstract states can load layouts to.

We'll look into organization of each of these folders in more detail when we
examine organization of the third level in the next post.

The rest of the folders located at the second level of the application root
have either a standard package-managed (e.g. `bower_components`, `node_modules`)
or flexible organization, so we won't be covering them here.

As always, please feel free to comment on this post below.

<div id="post-navigation" >
  <div class="previous">
    {% if page.previous.url %}
    <a href="{{page.previous.url}}" title="Previous post: {{page.next.title}}">
    <i class="fa fa-lg fa-arrow-circle-left"></i>
    {{page.previous.title}}
    </a>
    {% endif %}
  </div>
  <div class="next">
    {% if page.next.url %}
    <a href="{{page.next.url}}" title="Next post:
    {{page.next.title}}">NEXT: {{page.next.title}} <i class="fa fa-2x fa-arrow-circle-right"></i></a>
    {% endif %}
  </div>
</div>

___

Today my environment was:

- Angular 1.3.5
- AngularUI Router 0.2.12
- Mac OS X 10.10.1
