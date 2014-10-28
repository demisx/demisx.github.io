---
layout: post
title:  "Testing AngularJS child directive that requires parent directive"
date:   2014-10-14 21:45:00
categories: angularjs, unit testing
---

In AngularJS we can have one or more directives communicate with each other. This 
gives us ability to create web components (hello AngularJS 2.0) that are built 
from multiple directives. In order to for a directive A to access the API methods 
defined in a controller of a directive B, the latter needs to include the `require:` 
property with a value set to the name of directive A. The compilation of 
directive B will fail if controller of directive A is not found in the "search
path".

<div class="alert alert-warning">
<i class="fa fa-bell-o fa-2x"></i>
  Please note, when using `require` property, the directives are sharing 
  <b>the same instance</b> of the controller.
</div>

Here is an example of a directive B looking for directive A controller on the
same or parent DOM elements by prefixing directive name with `^`:

```coffeescript
angular.module 'app.directives', []

.directive 'directiveA', ->
  restrict: 'E'
  controller: ->
    @add = (x,y) ->
      x+y
  return
  
.directive 'directiveB', ->
  restrict: 'E'
  require: '^directiveA' # search for directive A controller on the same or parent elements
  link: (scope, elem, attrs, ctrl) ->
    # let's define some function on directive B scope 
    # that leverages business logic of directive A
    scope.letsAdd = (x,y) ->
      ctrl.add x,y
    return
```

And let's assume these directives have simple  "parent-child" relationship in HTML:

```html
<directive-a>
  <directive-b></directive-b>
</directive-a>
```

The issue comes in when we want to unit test directive B in isolation, without 
compiling directive A and everything that may come with it. 
If we try to test directive B just as a standalone directive, the compilation 
will fail with a `Controller 'directiveA', required by directive 'directiveB', can't be found!`
error as expected due to the `require: ^directiveA` property.

To fix this error, we need to mock the directive A controller and then add it 
to the data store of some parent DOM element, e.g. `<fake-parent>` (see below).
We don't care what this parent is, as long as we can put controller mock into
its data strore. 

Note, the  naming convention for the controller key is 

`'$' + name of directive + 'Controller'`. 

Thus, for Angular to find a mocked controller for the directive A, we'd need to 
associate the mock function with `$directiveAController` key in the element data store:

```coffeescript
  it 'ensures letsAdd() calls parent directory `add` method', ->
    module 'directives.cdSlideviewer'

    # mock directive A controller and spy on 'add' method
    directiveAControllerMock =
      add: ->
        123 # you can return anything here, since we are not unit testing it
    
    mySpy = sinon.spy directiveAControllerMock, 'add'

    inject ($compile, $rootScope) ->
      elem = angular.element '<fake-parent><directive-b><directive-b></fake-parent>'
      elem.data '$directiveAController', directiveAControllerMock
      $compile(elem) $rootScope.$new()
      elem = elem.find 'directive-b'
      scope = elem.scope()
      scope.letsAdd 3, 4 # call method under test
      return

    # assert that our spy was called with proper arguments
    expect(mySpy).to.have.been.calledWith 3, 4 # 
    return
```


___
##### Today my environment was:

- AngularJS 1.3.0
- Mocha 1.21.4 / Chai 1.9.2
- Gulp.js 3.8.8
- Node.js 0.10.31
