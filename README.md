# Components
#### Components only control their _own_ View and Data
* Never modify data or DOM outside their own scope.
* Modifying creates side-effects that lead to chaos.
* Therefore, Angular components _always_ use isolate scope.
#### Components have well-defined public API - Inputs and Outputs
* Inputs: use `<` and `@` bindings only.
* Never change property of passed in object or array.
* Outputs: use `&` for component event callbacks.
* Pass data to callback through param map {key : value}.
#### Components have well-defined lifecycle
* `$onInit` - controller initialization code.
* `$onChanges(changeObj)` - called whenever one-way bindings are updated.
    * changeObj.currentValue
    * changeObj.previousValue
* `$postLink` - similar to 'link' in directive.
* `$onDestroy` - when scope is about to be destroyed.
#### Application is a tree of components
* Entire application should be comprised of components.
* Each one would have a well-defined input and output.
* 2-way data binding is minimized as much as possible.

## Steps to create Components
#### Step 1: Register Component With Module
```
angular.module('App', [])
.component('myComponent', {
    templateUrl: 'template.html',
    controller: CompController,
    bindings: {
        prop1: '<',
        prop2: '@',
        onAction: '&'
    }
});
```
`'myComponent'` - normalized form. In HTML use `my-component`.

`.component('myComponent', {...});` - simple config object, NOT a function.
#### Step 2: Configure Component
```
angular.module('App', [])
.component('myComponent', {
    templateUrl: 'template.html',
    controller: CompController,
    bindings: {
        prop1: '<',
        prop2: '@',
        onAction: '&'
    }
});
```
`templateUrl: 'template.html'` - almost always have a template.

`controller: CompController` - not required. Empty function provided automatically. Placed on scope with label `$ctrl`.

`bindings: {...}` - 'bindings' object is the isolate scope param mapping definition.

#### Step 3: Reference Props in Template
```
<div
    ng-click="$ctrl.onAction({myArg: 'val'})">
    {{ $ctrl.prop1.prop }} and {{ $ctrl.prop2 }}
</div>
```
#### Step 4: Use Component in HTML
```
<my-component
    prop1="val-1"
    prop2="@parentProp"
    on-action="parentFunction(myArg)">

    {{ $ctrl.prop1.prop }} and {{ $ctrl.prop2 }}
</my-component>
```
***
#### _Summary_
* Components encourage component-based architecture, but they don't enforce it 100%, so we must follow conventions.
* Components should never modify data or DOM that doesn't belong to them. That's why it always has isolate scope and well-defined API.
* Register component with `.component('name', configObj)`.
* Provide controller only if you are adding extra functionality. Otherwise, Angular already provides an empty function for us.
***