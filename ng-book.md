##Scope
- The scopes of the application refer to the application model. Scopes are the execution context for expressions. The `$scope` object is where we define the business functionality of the application, the methods in our controllers, and properties in the views.
- Scopes serve as the **glue** between the controller and the view. Just before our app renders the view to the user, the view template links to the scope, and the app sets up the DOM to notify Angular for property changes.
- $scopes in AngularJS are arranged in a hierarchical structure that mimics the DOM and thus are nestable: We can reference properties on parent $scopes

###The $scope View of the World
- When Angular starts to run and generate the view, it will create a binding from the root ng-app element to the $rootScope. The $rootScope is the eventual parent of all $scope objects.

##Controllers
- Controllers in AngularJS exist to augment the view of an AngularJS application.
- One major distinction between AngularJS and other JavaScript frameworks is that the controller is not the appropriate place to do any DOM manipulation or formatting, data manipulation, or state maintenance beyond holding the model data. It is simply the glue between the view and the $scope model.

##Controller Hierarchy (Scopes Within Scopes)
- Every part of an AngularJS application has a parent scope ( as we've seen, at the ng-app level, this scope is called the $rootScope), regardless of the context within which it is rendered.
- With the exception of isolate scopes, all scopes are created with prototypal inheritance, meaning that they have access to their parent scopes.
- By default, for any property that AngularJS cannot find on a local scope, AngularJS will crawl up to the containing (parent) scope and look for the property or method there. If AngularJS can't find the property there, it will walk to that scope's parent and so on and so forth until it reaches the controllers.
