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
    <th class="text-center">Use case</th>
    <th class="text-center">Component</th>
    <th class="text-center">Sub-component</th>
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
        <li>Shareable data</li>
      </ol>
    </td>
    <td>Factory</td>
    <td></td>
    <td>
      md5Service<br/>
      authService
    </td>
    <td>
      These are generally refered to as `application services` or simply `services`.
      Do not confuse it with the name of AngularJS `Service` component that we've 
      excluded from this post`.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Reusable application business logic that <u>does&nbsp;need</u> to be 
        configured in `module.config` phase before it can be used</li>
      </ol>
    </td>
    <td>Provider</td>
    <td></td>
    <td>
      facebookLoginService<br/>
      geoService
    </td>
    <td>Examples of such configurations may include setting Facebook application id or
        an API key for accessing geolocation service.
    </td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Models that map to database tables</li>
        <li>CRUD interaction with `RESTful` server-side data sources</li>
        <li>Business logic <u>specific to the model</u> </li>
      </ol>
    </td>
    <td>Factory</td>
    <td>$resource</td>
    <td>
      User<br/>
      Session<br/>
      CreditCard</td>
    <td></td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Wire up (e.g. initialize) scope with data and methods for views to use </li>
      </ol>
    </td>
    <td>Controller</td>
    <td></td>
    <td>
      DashboardController<br/>
      ProjectController<br/>
      SlideController
    </td>
    <td></td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Manupulations of DOM template that you want all directive instances to inherit</li>
      </ol>
    </td>
    <td>Directive</td>
    <td>compile<br/> function</td>
    <td></td>
    <td>Rarely used<sup>1</sup>. Does not have access to scope.</td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Manupulations of DOM instances in linked HTML</li>
        <li>Addition of DOM event listeners</li>
        <li>Directive specific scope watchers and/or attribute observers</li>
      </ol>
    </td>
    <td>Directive</td>
    <td>link<br/> function</td>
    <td></td>
    <td>By link function I mean `post-link` function</td>
  </tr>
  <tr>
    <td>
      <ol>
        <li>Business logic <u>specific to the directive</u></li>
        <li>API methods for communicating between directives</li>
      </ol>
    </td>
    <td>Directive</td>
    <td>controller</td>
    <td></td>
    <td>
      A directive wishing to access controller methods of another directive
      needs to explicitly require it
    </td>
  </tr>
  <tr>
    <td>
    <ol>
      <li>Transform model data without changing the source data</li>
      <li>Filter out ng-repeats based on a certain criteria</li>
    </ol>
    </td>
    <td>Filter</td>
    <td></td>
    <td>
      displayName<br/>
      startsWith<br/>
      sortBy
      pluralize
    </td>
    <td></td>
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
    <td></td>
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
    <td></td>
    <td>
      appConfig<br/>
      servicesConfig
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
    <td></td>
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