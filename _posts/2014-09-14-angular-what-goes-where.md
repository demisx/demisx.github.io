---
layout: post
title:  "AngularJS 1.x WGW (What Goes Where) guide"
date:   2014-09-14 6:45:00
categories: angularjs
---

A few months ago, I finally had a chance to get my hands on the so-much-talked-about
[AngularJS](https://angularjs.org/) framework and I really digged it. In fact,
I like it so much that I am 100% sure this framework will stay ahead of others
for a long time. Well, in our world this means for at least another year or two.

<!--more-->

Though, I've got the basics down fairly fast, I had hard time around understanding
the proper code organization. Being a perfectionist didn't help either. Coming from a very
opiniated Rails framework it didn't feel comfortable at first that AngularJS gave
significantly more freedom to do things. But thank God the "Rails magic" is gone!

The type of questions that kept poping up  in my head were:

* "OK, I want to code this functionality, but where shoud I really put it"?
* Should I stick this code into a View controller, or should I put it into a Service?
* Perhaps, a directive could be a good place for it, but then where exactly in a directive?

Doing my googling I've realized there were plenty of other pilligrims like me
trying to find their way in this new and exciting AngularJS world.

So, I am putting together this cheatsheet hoping it will guide us on where the stuff
should really go. It's not meant to be detail, but it should give you the right
starting point depending on what you are trying to achieve.


<div class="alert alert-warning">
<i class="fa fa-bell-o fa-2x"></i>
  *Please note, that I've intentionally omitted the `Service` component from this list.
  I find that using factories lets me accomplish everything I could do with
  `Service`, therefore I really had no real need to engage the latter. `Service`
  component does have some limited benefits if you are using CoffeeScript classes,
  but this would out of the scope of this post. Let's KISS (Keep It Simple Stupid).*
</div>

<style>
  table#angular-table > tbody > tr > td:nth-child(1) {
    width: 30%;
    padding-left: 20px;
  }
  table#angular-table > tbody > tr > td:nth-child(1) > ol {
    padding-left: 20px;
  }
  table#angular-table > tbody > tr > td li {
    padding-bottom: 5px;
  }
  table#angular-table > tbody > tr > td:nth-child(2),
  table#angular-table > tbody > tr > td:nth-child(3) {
    font-weight: bold;
    text-align: center;
  }
</style>

<table id="angular-table" class="table table-striped table-bordered">
  <tr>
    <th class="text-center">Use Case</th>
    <th class="text-center">App Component</th>
    <th class="text-center">Angular Component</th>
    <th class="text-center">Examples</th>
    <th class="text-center">Notes</th>
  </tr>
  <tr>
    <td>
      <ol>
        <li>
         Reusable application-wide business logic that <u>does&nbsp;not&nbsp;need</u>
         to be further configured in `module.config` phase
        </li>
        <li>Application-wide shareable data</li>
      </ol>
    </td>
    <td>Non-configurable Application Service</td>
    <td>Factory</td>
    <td>
      md5Service<br/>
      authService
    </td>
    <td>
      These are generally referred to as `application services` or simply `services`.
      Do not confuse it with the name of AngularJS `Service` component that we've
      excluded from this post for simplicity.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Reusable application-wide business logic that <u>does&nbsp;need</u> to be
        configured in `module.config` phase before it can be used</li>
      </ol>
    </td>
    <td>Configurable Application Service</td>
    <td>Provider</td>
    <td>
      facebookLoginProvider<br/>
      geoProvider
    </td>
    <td>Examples of such configurations may include setting Facebook application id or
        an API key for accessing geo-location service.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>View specific business logic</li>
      </ol>
    </td>
    <td>View Service</td>
    <td>Factory</td>
    <td>
      dashboardViewService<br/>
      adListViewService
    </td>
    <td>
      Keeping view logic in application controllers makes them eventually fat. Recommend
      placing it in the view service instead and have controllers simply call these
      view service methods and process their responses.
    </td>
  </tr>
   <tr>
      <td>
      <ol>
        <li>Display model data in UI differently from the actual source data</li>
        <li>Filter out ng-repeats based on a certain criteria</li>
      </ol>
      </td>
      <td>View Filter</td>
      <td>Filter</td>
      <td>
        currencyFilter<br/>
        startsWithFilter<br/>
        sortByFilter<br/>
        pluralizeFilter
      </td>
      <td></td>
    </tr>
  <tr>
    <td>
      <ol>
        <li>Models that map to database tables</li>
        <li>CRUD interaction with `REST-ful` server-side data sources</li>
        <li>Business logic <u>specific to the model</u> </li>
      </ol>
    </td>
    <td>Model</td>
    <td>Factory ($resource)</td>
    <td>
      User<br/>
      Session<br/>
      CreditCard</td>
    <td>
      For anything more than a trivial implementation, I recommend using<br/>
      <a href="www.js-data.io/docs/js-data-angular" target="_blank">js-data-angular</a> instead of
      the built-in `$resource`.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Wire up (e.g. initialize) scope with data and methods for views to use </li>
      </ol>
    </td>
    <td>Application Controller</td>
    <td>Controller</td>
    <td>
      DashboardController<br/>
      ProjectController<br/>
      SlideController
    </td>
    <td>
      Keep controllers thin. Should mostly contain calls to methods in an application service or
      in a view service.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Manipulations of DOM template that you want all directive instances to inherit</li>
      </ol>
    </td>
    <td>DOM Template Manipulator</td>
    <td>Directive<br/>(compile function)</td>
    <td></td>
    <td>Rarely used<sup>1</sup>. Does not have access to scope.</td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Manipulations of DOM instances in linked HTML</li>
        <li>Addition of DOM event listeners</li>
        <li>Directive specific scope watchers and/or attribute observers</li>
      </ol>
    </td>
    <td>DOM Instance Manipulator</td>
    <td>Directive (link function)</td>
    <td></td>
    <td>By link function I mean the `post-link` function</td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Business logic <u>specific to the directive</u></li>
        <li>API methods for communicating between directives</li>
      </ol>
    </td>
    <td>DOM Service/API</td>
    <td>Directive (controller)</td>
    <td></td>
    <td>
      A directive controller can be written inline as part of DDO or injected via DI. Another directive
      wishing to access controller methods of a given directive needs to explicitly require it.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Editable application-wide settings</li>
        <li>Editable application-wide value objects or primitives
            that controller or service components need access to.
        </li>
      </ol>
    </td>
    <td>Value</td>
    <td>Value</td>
    <td>
      uiConfig<br/>
      currentUser</td>
    <td><u>Cannot be</u> injected into the
      `module.config` phase, but can be altered by a `Decorator`.
    Examples could include an object that tracks currently logged in user
    properties or any other object, properties of which you want to access in
    various parts of your application</td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Constant application-wide settings</li>
      </ol>
    </td>
    <td>Constant</td>
    <td>Constant</td>
    <td>
      FACEBOOK_ACCOUNT<br/>
      AUTH_EVENTS
    </td>
    <td>Besides controller and a service, Constant <u>can also be injected</u>
        into the `module.config` phase. Constants cannot be altered by a
        `Decorator`. Avoid modifying the Constant. If you need an editable object
        or value, use `Value` component/provider instead.
    </td>
  </tr>
   <tr>
    <td>
      <ol>
        <li>Augment/tweak some-third party service, while leaving the service mostly intact</li>
        <li>Initialize a value in a `Value` component where such initialization
            requires access to a factory service</li>
      </ol>
    </td>
    <td>Decorator</td>
    <td>Decorator</td>
    <td>myLog</td>
    <td>
      If you simply want to replace an instance, then using `Factory` or `Value`
      component is musch simpler
    </td>
  </tr>
</table>

___
<sup>1</sup> See [example](http://stackoverflow.com/questions/13852248/how-to-write-a-double-and-a-ntimes-directive-for-angularjs/13873098#13873098)
of how a compile function can be used.

<a class="twitter-follow-button"
  href="https://twitter.com/demisx1"
  data-show-count="false"
  data-lang="en"
  data-size="large">
</a> for more tips and best practices.
