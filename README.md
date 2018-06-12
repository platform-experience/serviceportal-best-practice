# Service Portal: AngularJS Style Guide

## Purpose

A sensible & opinionated AngularJS style guide for teams using Service Portal.

*Based off of the [Angular 1 Style Guide](https://github.com/johnpapa/angular-styleguide/tree/master/a1) by [John Papa](https://johnpapa.net/) and up-to-date with AngularJS 1.5 components.*

> Note: The original version of Angular is officially called AngularJS; this includes all versions of Angular starting with 1. The current version of Angular is simply called Angular, which includes versions 2 and beyond and will be referred to as such in this guide.

## Table of Contents

  1. [IIFE](#iife)
  1. [Modules](#modules)
  1. [Named Controllers](#named-controllers)
  1. [Controller as Syntax](#controller-as-syntax)
  1. [Bindable Members at Top](#bindable-members-at-top)
  1. [$onInit](#oninit)
  1. [Components](#components)
  1. [One-way Binding](#one-way-binding)
  1. [Defer Logic to Services](#defer-logic-to-services)
  1. [Small Functions](#small-functions)

## IIFE

  Where possible, wrap components in an (IIFE) Immediately Invoked Function Expression. This removes variables from the global namespace, thus preventing variable collisions and providing an enclosed scope.

  ```javascript
  (function() {
    /* */
  })();
  ```

**[Back to top](#table-of-contents)**

## Modules

  Set a module only once in its own file. Why should I do this?

  John Papa answers this best in his style guide.

  > Separates configuration from module definition, components, and active code. Provides an identifiable place to set configuration for a module.

  Do this to define a module.

  ```javascript
  angular.module('pe-timeline', []);
  ```

  Get instances of the module this way.

  ```javascript
  angular.module('pe-timeline');
  ```

  Read more on the topic, from John Papa.

  [https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#modules-1](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#modules-1)

**[Back to top](#table-of-contents)**

## Named Controllers

  Use named controllers instead of anonymous functions. Why named controllers?

   * easier to debug
   * creates more readable code

  Also, be sure to append the controller name with the suffix Controller. This is a good practice that is commonly used and is descriptive.

  ```javascript
  function PeopleCardController() {
    var c = this;
  }
  ```

  The platform provides you with anonymous functions by default when creating widgets, but if possible try to avoid this.

  ```javascript
  function() {
    var c = this;
  }
  ```

**[Back to top](#table-of-contents)**

## Controller as Syntax

  The Controllers as Syntax was introduced in AngularJS 1.2. Use this syntax instead of `$scope` inside your controller. Why Controller as?

   * it is a common and best practice
   * removes scope inheritance issues
   * eliminates having to inject `$scope` as a dependency
   * `$scope` is phased out in Angular

  Every time you create a new widget the platform provides this syntax for you by default, which is nice.

  ```javascript
  function() {
    var c = this;
    c.isVisible = true;
  }
  ```

  Try to avoid this, if possible.

  ```javascript
  function($scope) {
    $scope.isVisible = true;
  }
  ```

**[Back to top](#table-of-contents)**

## Bindable Members at Top

  Place bindable members at the top of the context in alphabetical order. This makes it easy to read and at a glance you know exactly which members are bound to the view and where to find the implementation details.

  ```javascript
  function FeedbackCardController() {
    var c = this;
    c.closeSurvey = closeSurvey;
    c.setRating = setRating;

    function closeSurvey() {
      /* */
    }

    function setRating() {
      /* */
    }
  }
  ```

**[Back to top](#table-of-contents)**

## $onInit

  AngularJS 1.5 brings the `$onInit` method. This is a good place to put initialization code for your controller. From the documentation:

  > Called on each controller after all the controllers on an element have been constructed and had their bindings initialized (and before the pre & post linking functions for the directives on this element).

  ```javascript
  function TabSelectionController() {
    var c = this;

    c.$onInit = function() {
      c.selection = 1;
      c.isVisible = false;
    };
  }
  ```

  Another great use case for utilizing `$onInit`, is when you need a place to fire off an immediately invoked function.

  ```javascript
  c.$onInit = function() {
    activateCharts();
    setDefaults();
  };
  ```

**[Back to top](#table-of-contents)**

## Components

  AngularJS 1.5 brings the new `.component()` helper method, which allows developers to write in a modern Angular style and makes upgrading to the new framework an easier task. Here are the advantages from the AngularJS documentation:

  > * simpler configuration than plain directives
  > * promote sane defaults and best practices
  > * optimized for component-based architecture
  > * writing component directives will make it easier to upgrade to Angular

  ```javascript
  (function() {
    'use strict';

    var hamburgerMenu = {
      template: [
        '<button type="button" data-target="#nav-bar">',
        '<span class="sr-only">${Toggle nav}</span>',
        '<span class="icon-bar"></span>',
        '<span class="icon-bar"></span>',
        '<span class="icon-bar"></span>',
        '</button>'
      ].join('')
    };

    angular
      .module('sec-ops')
      .component('hamburgerMenu', hamburgerMenu);
  })();
  ```

**[Back to top](#table-of-contents)**

## One-way Binding

  AngularJS 1.3 delivered a new performance enhancing feature called one-time binding. From the AngularJS documentation:

  > One-time expressions will stop recalculating once they are stable, which happens after the first digest if the expression result is a non-undefined value.

  ```html
  <div class="panel-heading">{{::options.title}}</div>
  ```

  The syntax can easily be applied inside an ng-repeat as well:

  ```html
  <ul>
    <li ng-repeat="user in ::c.users"></li>
  </ul>
  ```

  Note: Be careful using one-way bindings in areas where the data could fluctuate in the future.

**[Back to top](#table-of-contents)**

## Defer Logic to Services

  Defer controller logic to services or factories. Why you say?

   * keeps the controller focused and lean
   * promotes reuse via dependency injection
   * reduces repetitive controller logic
   * ideal for unit testing and mocking

   Create a service.

  ```javascript
  (function() {
    'use strict';

    function eventService() {
      var service = {
        getInitialEvents: getInitialEvents
      };
      return service;

      function getInitialEvents() {
        /* */
      }
    }
  })();
  ```

  Simply inject that service into the controller.

  ```javascript
  function TimelineController(eventService) {
    var c = this;

    c.$onInit = function() {
      c.initialEvents = eventService.getInitialEvents();
    };
  }
  ```

  Read more on the topic, from the man John Papa.

  [https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#defer-controller-logic-to-services](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#defer-controller-logic-to-services)

**[Back to top](#table-of-contents)**

## Small Functions

  Write small functions; less than 20 lines of code is ideal. Abstract large functions to smaller ones. Why?

   * produces more readable code
   * easier to maintain
   * promotes code reuse

  Note: Widget Server script abstraction can easily be achieved using methods from Script includes. Read more on Script includes from the ServiceNow documentation.

  [https://docs.servicenow.com/bundle/kingston-application-development/page/script/server-scripting/concept/c_ScriptIncludes.html](https://docs.servicenow.com/bundle/kingston-application-development/page/script/server-scripting/concept/c_ScriptIncludes.html)

**[Back to top](#table-of-contents)**