---
### Golden rule
---
* Common services should be outside of the app structure, it helps to re-write UI in future for various views(if required).
* Proper Indentation of the code so that it will be much more readable.
* Write custom directive for common html element across various functionality.
* Unit test cases should be written from beginning of the development across various functionalities using Jasmin/Karma and Protractor for end to end testing.
* Use NGDoc/JSDoc to generate a complete HTML documentation website.
* Use the AngularUI Router for client-side routing.
* Exception handling : Use a decorator at configuration time using the `$provide` service on the `$exceptionHandler` service to perform custom actions when exceptions occur.
* Code modularization principle to be followed which means separate files maintained for configuration, routing, views etc.
* Use `$document` and `$window` instead of document and window.
* Use `$timeout` and `$interval` instead of `setTimeout` and `setInterval`.
* Use consistent names for all controllers named after their feature. Use UpperCamelCase for controllers, as they are constructors.
* Append the controller name with the suffix Controller.
* Don’t put business logic in controller, it should be written in services.
----
### Single Responsibility
----
	
* Define 1 component per file.

 The following example defines the `app` module and its dependencies, defines a controller, and defines a factory all in the same file.

  ```javascript
  /* avoid */
  angular
      .module('app', ['ngRoute'])
      .controller('SomeController', SomeController)
      .factory('someFactory', someFactory);

  function SomeController() { }

  function someFactory() { }
  ```

  The same components are now separated into their own files.

  ```javascript
  /* recommended */

  // app.module.js
  angular
      .module('app', ['ngRoute']);
  ```

  ```javascript
  /* recommended */

  // some.controller.js
  angular
      .module('app')
      .controller('SomeController', SomeController);

  function SomeController() { }
  ```

  ```javascript
  /* recommended */

  // someFactory.js
  angular
      .module('app')
      .factory('someFactory', someFactory);

  function someFactory() { }
  ```

----
### IIFE (JavaScript Closures)
----

* Wrap Angular components in an Immediately Invoked Function Expression (IIFE).
* An IIFE removes variables from the global scope. This helps prevent variables and function declarations from living longer than expected in the global scope, which also helps avoid variable collisions.
* When your code is minified and bundled into a single file for deployment to a production server, you could have collisions of variables and many global variables. An IIFE protects you against both of these by providing variable scope for each file.

  ```javascript
  /* avoid */
  // logger.js
  angular
      .module('app')
      .factory('logger', logger);

  // logger function is added as a global variable
  function logger() { }

  // storage.js
  angular
      .module('app')
      .factory('storage', storage);

  // storage function is added as a global variable
  function storage() { }
  ```

  ```javascript
  /**
   * recommended
   *
   * no globals are left behind
   */

  // logger.js
  (function() {
      'use strict';

      angular
          .module('app')
          .factory('logger', logger);

      function logger() { }
  })();

  // storage.js
  (function() {
      'use strict';

      angular
          .module('app')
          .factory('storage', storage);

      function storage() { }
  })();
  ```

---
## Modules 
---

#### Avoid Naming Collisions

* Use unique naming conventions with separators for sub-modules.
* Unique names help avoid module name collisions. Separators help define modules and their submodule hierarchy. For example `app` may be your root module while `app.dashboard` and `app.users` may be modules that are used as dependencies of `app`.

#### Definitions (aka Setters)

* Declare modules without a variable using the setter syntax.
* With 1 component per file, there is rarely a need to introduce a variable for the module.

  ```javascript
  /* avoid */
  var app = angular.module('app', [
      'ngAnimate',
      'ngRoute',
      'app.shared',
      'app.dashboard'
  ]);
  ```

  Instead use the simple setter syntax.

  ```javascript
  /* recommended */
  angular
      .module('app', [
          'ngAnimate',
          'ngRoute',
          'app.shared',
          'app.dashboard'
      ]);
  ```

#### Getters

* When using a module, avoid using a variable and instead use chaining with the getter syntax.
* This produces more readable code and avoids variable collisions or leaks.

  ```javascript
  /* avoid */
  var app = angular.module('app');
  app.controller('SomeController', SomeController);

  function SomeController() { }
  ```

  ```javascript
  /* recommended */
  angular
      .module('app')
      .controller('SomeController', SomeController);

  function SomeController() { }
  ```

#### Setting vs Getting

* Only set once and get for all other instances.
* A module should only be created once, then retrieved from that point and after.

  ```javascript
  /* recommended */

  // to set a module
  angular.module('app', []);

  // to get a module
  angular.module('app');
  ```

#### Named vs Anonymous Functions

* Use named functions instead of passing an anonymous function in as a callback.
* This produces more readable code, is much easier to debug, and reduces the amount of nested callback code.

  ```javascript
  /* avoid */
  angular
      .module('app')
      .controller('DashboardController', function() { })
      .factory('logger', function() { });
  ```

  ```javascript
  /* recommended */

  // dashboard.js
  angular
      .module('app')
      .controller('DashboardController', DashboardController);

  function DashboardController() { }
  ```

  ```javascript
  // logger.js
  angular
      .module('app')
      .factory('logger', logger);

  function logger() { }
  ```

---
## Controllers
---

#### controllerAs View Syntax

* Use the `controllerAs` syntax over the `classic controller with $scope` syntax.
* Controllers are constructed, "newed" up, and provide a single new instance, and the `controllerAs` syntax is closer to that of a JavaScript constructor than the `classic $scope syntax`.
* It promotes the use of binding to a "dotted" object in the View (e.g. `customer.name` instead of `name`), which is more contextual, easier to read, and avoids any reference issues that may occur without "dotting".
* Helps avoid using `$parent` calls in Views with nested controllers.

  ```html
  <!-- avoid -->
  <div ng-controller="CustomerController">
      {{ name }}
  </div>
  ```

  ```html
  <!-- recommended -->
  <div ng-controller="CustomerController as customer">
      {{ customer.name }}
  </div>
  ```

#### controllerAs with vm

* Use a capture variable for `this` when using the `controllerAs` syntax. Choose a consistent variable name such as `vm`, which stands for ViewModel.
* The `this` keyword is contextual and when used within a function inside a controller may change its context. Capturing the context of `this` avoids encountering this problem.

  ```javascript
  /* avoid */
  function CustomerController() {
      this.name = {};
      this.sendMessage = function() { };
  }
  ```

  ```javascript
  /* recommended */
  function CustomerController() {
      var vm = this;
      vm.name = {};
      vm.sendMessage = function() { };
  }
  ```

#### Bindable Members Up Top

* Place bindable members at the top of the controller, alphabetized, and not spread through the controller code.
* Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View.
* Setting anonymous functions in-line can be easy, but when those functions are more than 1 line of code they can reduce the readability. Defining the functions below the bindable members (the functions will be hoisted) moves the implementation details down, keeps the bindable members up top, and makes it easier to read.

  ```javascript
  /* avoid */
  function SessionsController() {
      var vm = this;

      vm.gotoSession = function() {
        /* ... */
      };
      vm.refresh = function() {
        /* ... */
      };
      vm.search = function() {
        /* ... */
      };
      vm.sessions = [];
      vm.title = 'Sessions';
  }
  ```

  ```javascript
  /* recommended */
  function SessionsController() {
      var vm = this;

      vm.gotoSession = gotoSession;
      vm.refresh = refresh;
      vm.search = search;
      vm.sessions = [];
      vm.title = 'Sessions';

      ////////////

      function gotoSession() {
        /* */
      }

      function refresh() {
        /* */
      }

      function search() {
        /* */
      }
  }
  ```
---
 ## Services
---

#### Singletons


* Services are instantiated with the `new` keyword, use `this` for public methods and variables. Since these are so similar to factories, use a factory instead for consistency.


  ```javascript
  // service
  angular
      .module('app')
      .service('logger', logger);

  function logger() {
    this.logError = function(msg) {
      /* */
    };
  }
  ```


#### Function Declarations to Hide Implementation Details


* Use function declarations to hide implementation details. Keep your accessible members of the factory up top. Point those to function declarations that appears later in the file.
* Placing accessible members at the top makes it easy to read and helps you instantly identify which functions of the factory you can access externally.
* Placing the implementation details of a function later in the file moves that complexity out of view so you can see the important stuff up top.
* Function declaration are hoisted so there are no concerns over using a function before it is defined (as there would be with function expressions).
* You never have to worry with function declarations that moving `var a` before `var b` will break your code because `a` depends on `b`.
* Order is critical with function expressions

  ```javascript
  /**
   * avoid
   * Using function expressions
   */
   function dataservice($http, $location, $q, exception, logger) {
      var isPrimed = false;
      var primePromise;

      var getAvengers = function() {
          // implementation details go here
      };

      var getAvengerCount = function() {
          // implementation details go here
      };

      var getAvengersCast = function() {
         // implementation details go here
      };

      var prime = function() {
         // implementation details go here
      };

      var ready = function(nextPromises) {
          // implementation details go here
      };

      var service = {
          getAvengersCast: getAvengersCast,
          getAvengerCount: getAvengerCount,
          getAvengers: getAvengers,
          ready: ready
      };

      return service;
  }
  ```

  ```javascript
  /**
   * recommended
   * Using function declarations
   * and accessible members up top.
   */
  function dataservice($http, $location, $q, exception, logger) {
      var isPrimed = false;
      var primePromise;

      var service = {
          getAvengersCast: getAvengersCast,
          getAvengerCount: getAvengerCount,
          getAvengers: getAvengers,
          ready: ready
      };

      return service;

      ////////////

      function getAvengers() {
          // implementation details go here
      }

      function getAvengerCount() {
          // implementation details go here
      }

      function getAvengersCast() {
          // implementation details go here
      }

      function prime() {
          // implementation details go here
      }

      function ready(nextPromises) {
          // implementation details go here
      }
  }
  ```


---
## Data Services
---

#### Separate Data Calls
 

* Refactor logic for making data operations and interacting with data to a factory. Make data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.
* The controller's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the controller be simpler and more focused on the view.
* This makes it easier to test (mock or real) the data calls when testing a controller that uses a data service.
* Data service implementation may have very specific code to handle the data repository. This may include headers, how to talk to the data, or other services such as `$http`. Separating the logic into a data service encapsulates this logic in a single place hiding the implementation from the outside consumers (perhaps a controller), also making it easier to change the implementation.

  ```javascript
  /* recommended */

  // dataservice factory
  angular
      .module('app.core')
      .factory('dataservice', ['$http', 'logger' , dataservicefn]);


  function dataservicefn($http, logger) {
      return {
          getChart: getChart
      };

      function getChart() {
          return $http.get('/api/maa')
              .then(getChartComplete)
              .catch(getChartFailed);

          function getChartComplete(response) {
              return response.data.results;
          }

          function getChartFailed(error) {
              logger.error('XHR Failed for getChart.' + error.data);
          }
      }
  }
  ```
---
## Directives
---
### Limit 1 Per File

* Create one directive per file. Name the file for the directive.
* It is easy to mash all the directives in one file, but difficult to then break those out so some are shared across apps, some across modules, some just for one module.
* One directive per file is easy to maintain.

 

  ```javascript
  /* avoid */
  /* directives.js */

  angular
      .module('app.widgets')

      /* order directive that is specific to the order module */
      .directive('orderCalendarRange', orderCalendarRange)

      /* sales directive that can be used anywhere across the sales app */
      .directive('salesCustomerInfo', salesCustomerInfo)

      /* spinner directive that can be used anywhere across apps */
      .directive('sharedSpinner', sharedSpinner);

  function orderCalendarRange() {
      /* implementation details */
  }

  function salesCustomerInfo() {
      /* implementation details */
  }

  function sharedSpinner() {
      /* implementation details */
  }
  ```

  ```javascript
  /* recommended */
  /* calendar-range.directive.js */

  /**
   * @desc order directive that is specific to the order module at a company named Acme
   * @example <div acme-order-calendar-range></div>
   */
  angular
      .module('sales.order')
      .directive('acmeOrderCalendarRange', orderCalendarRange);

  function orderCalendarRange() {
      /* implementation details */
  }
  ```

  ```javascript
  /* recommended */
  /* customer-info.directive.js */

  /**
   * @desc sales directive that can be used anywhere across the sales app at a company named Acme
   * @example <div acme-sales-customer-info></div>
   */
  angular
      .module('sales.widgets')
      .directive('acmeSalesCustomerInfo', salesCustomerInfo);

  function salesCustomerInfo() {
      /* implementation details */
  }
  ```

  ```javascript
  /* recommended */
  /* spinner.directive.js */

  /**
   * @desc spinner directive that can be used anywhere across apps at a company named Acme
   * @example <div acme-shared-spinner></div>
   */
  angular
      .module('shared.widgets')
      .directive('acmeSharedSpinner', sharedSpinner);

  function sharedSpinner() {
      /* implementation details */
  }
  ```

  ### Provide a Unique Directive Prefix

* Provide a short, unique and descriptive directive prefix such as `mcfSalesCustomerInfo` which would be declared in HTML as `mcf-sales-customer-info`.
* The unique short prefix identifies the directive's context and origin.
* Avoid `ng-` as these are reserved for Angular directives. Research widely used directives to avoid naming conflicts, such as `ion-` for the  Ionic Framework 

### Restrict to Elements and Attributes

  - When creating a directive that makes sense as a stand-alone element, allow restrict `E` (custom element) and optionally restrict `A` (custom attribute). Generally, if it could be its own control, `E` is appropriate. General guideline is allow `EA` but lean towards implementing as an element when it's stand-alone and as an attribute when it enhances its existing DOM element. 
* It makes sense.
* While we can allow the directive to be used as a class, if the directive is truly acting as an element it makes more sense as an element or at least as an attribute.


  ```html
  <!-- avoid -->
  <div class="my-calendar-range"></div>
  ```

  ```javascript
  /* avoid */
  angular
      .module('app.widgets')
      .directive('myCalendarRange', myCalendarRange);

  function myCalendarRange() {
      var directive = {
          link: link,
          templateUrl: '/template/is/located/here.html',
          restrict: 'C'
      };
      return directive;

      function link(scope, element, attrs) {
        /* */
      }
  }
  ```

  ```html
  <!-- recommended -->
  <my-calendar-range></my-calendar-range>
  <div my-calendar-range></div>
  ```

  ```javascript
  /* recommended */
  angular
      .module('app.widgets')
      .directive('myCalendarRange', myCalendarRange);

  function myCalendarRange() {
      var directive = {
          link: link,
          templateUrl: '/template/is/located/here.html',
          restrict: 'EA'
      };
      return directive;

      function link(scope, element, attrs) {
        /* */
      }
  }
  ```