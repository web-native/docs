# Reflex Guide

## Installation

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/reflex/dist/main.js"></script>

<script>
// The above tag loads Reflex into a global "WebNative" object.
const Reflex = window.WebNative.Reflex;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/reflex
```

#### Import

Reflex is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

Reflex works both in browser and server environments.

```javascript
// Node-style import
import Reflex from '@web-native-js/reflex';

// Standard JavaScript import. (Actual path depends on where you installed Reflex to.)
import Reflex from './node_modules/@web-native-js/reflex/src/index.js';
```

## API

Reflex provides two types of reactivity: reflex actions and events.

“A reflex, or reflex action, is an involuntary and nearly instantaneous movement in response to a stimulus” – Wikipedia. Reflex follows this concept of triggering “reflexes” when an object is being modified or accessed.

### Reflex.observe

This method is used to observe changes to an object or array. Used in conjunction with this method are the `Reflex.set` and `Reflex.deleteProperty` mutation methods which we will cover shortly.

#### Syntax

```javascript
// Observe all properties
Reflex.observe(object, observer[, params = {}]);

// Observe a specific property
Reflex.observe(object, propertyName, observer[, params = {}]);

// Observe a list of properties
Reflex.observe(object, propertyNames, observer[, params = {}]);
```

Above, object represents an object or array; and observer represents a callback function that receives the change notifications.

#### Observing All Properties

```javascript
let observer = (newValues, oldValues, event) => {
    console.log(newValues, oldValues, event);
};
let obj = {};
Reflex.observe(obj, observer);
```

The code above will report changes as the object gets modified on any of its properties.

```javascript
let arr = [];
Reflex.observe(arr, observer);
```

The code above will report changes as the array gets modified with entries.

Let’s now effect a change and watch the console.

```javascript
Reflex.set(obj, ‘fruit’, ‘apple’);
```

Here’s what we expect to see:

```text
// newValues and oldValues
newValues                    {fruit:’apple’}
oldValues                    {fruit:undefined}
// event details
event.type                    ‘set’
event.target                <reference to obj>
event.fields                [‘fruit’]
event.created                [‘fruit’]
event.data                    {fruit:’apple’}
event._data                    {fruit:undefined}
```

#### Observing a Specific Property

```javascript
let observer = (newValue, oldValue, event) => {
    console.log(newValue, oldValue, event);
};

let obj = {};
Reflex.observe(obj, ‘fruit’, observer);
```

The code above will report changes as a “fruit” property gets created, modified or deleted on the object.

```javascript
let arr = [];
Reflex.observe(arr, 0, observer);
```

The code above will report changes as the array’s first entry gets created, modified or deleted.

Let’s effect a change and watch the console.

```javascript
Reflex.set(obj, ‘fruit’, ‘apple’);
```

Here’s what we expect to see:

```text
// newValues and oldValues
newValue                    ’apple’
oldValue                    undefined
// event details
event.type                    ‘set’
event.target                <reference to obj>
event.fields                [‘fruit’]
event.created                [‘fruit’]
event.data                    {fruit:’apple’}
event._data                    {fruit:undefined}
```

#### Observing a List of Properties

```javascript
let observer = (newValuesList, oldValuesList, event) => {
    console.log(newValuesList, oldValuesList, event);
};

let obj = {};
Reflex.observe(obj, [‘fruit’, ‘brand’], observer);
```

The code above will report changes as any of “fruit” or “brand” properties get created, modified or deleted on the object.

```javascript
let arr = [];
Reflex.observe(arr, [0, 3], observer);
```

The code above will report changes as the array’s first or fourth entry gets created, modified or deleted.

Now, we’ll effect a change on the first observed property and watch the console.

```javascript
Reflex.set(obj, ‘fruit’, ‘orange’);
```

Here’s what we expect to see:

```text
// newValues and oldValues
newValuesList                [’orange’, undefined]
oldValuesList                [undefined, undefined]
// event details
event.type                    ‘set’
event.target                <reference to obj>
event.fields                [‘fruit’]
event.created                [‘fruit’]
event.data                    {fruit:’orange’}
event._data                    {fruit:undefined}
```

Next, we’ll effect a change on the second observed property and watch the console.

```javascript
Reflex.set(obj, ‘brand’, ‘apple’);
```

Here’s what we expect to see:

```text
// newValues and oldValues
newValuesList                [’orange’, ’apple’]
oldValuesList                [’orange’, undefined]
// event details
event.type                    ‘set’
event.target                <reference to obj>
event.fields                [‘brand’]
event.created                [‘brand’]
event.data                    {brand:’apple’}
event._data                    {brand:undefined}
```

#### Constraining an Observer

The `params.type` parameter can be used to constrain an observer to respond to a specific mutation type like “set” and “del”.

```javascript
Reflex.observe(object, propertyName, observer, {type:’del’});
```

#### Tagging an Observer

The `params.tag` parameter can be used to tag an observer. This tag isn’t used specially except for identifying the observer. A tag can be anything: _string_, _number_, _object_, etc.

```javascript
Reflex.observe(object, propertyName, observer, {tag:’#tag’});
```

#### Return Value

`Reflex.observe` returns an Observer object that acts as a reference to the binding just made. The Observer object’s methods could be useful in other ways.

```javascript
let observerObject = Reflex.observe(object, observer);
// Synthetically fire the observer
observerObject.fire(eventInstance);

// Disconnect the observer
observerObject.disconnect();
```

#### Observing Deep Changes

It is possible to observe changes on nested objects or arrays.

Deep changes are detected when a change made on a nested object bubbles up the tree. These nested objects or arrays just need to be linked by Reflex to the parent object. This is automatically achieved when setting a child object to a parent object using the `Reflex.set` function. For an object tree that is already built, descendant objects could be “all linked-up” using the `Reflex.build` or `Reflex.link` support functions. \(These functions are described in detail shortly.\)

**Syntax**

```javascript
// Observe changes on both direct properties and descendant properties 
// We employ the params.observeDown parameter
Reflex.observe(object, observer, {observeDown:true});

// Observe changes on a specific path
// We use the dot (.) notation to represent our path
Reflex.observe(object, ‘level1.level2’, observer);

// Observe changes on a list of paths
// We provide an array of paths
Reflex.observe(object, [‘level1.level2_a’, ‘level1,level2_b’], observer);

// Observe changes on a wildcard path
// We provide a “template” path expression that could automatically match more than one path
Reflex.observe(object, ‘level1..level3’, observer);
```

**Using the params.observeDown Parameter**

When we observe down an object tree, deep changes will bubble to the handler bound up the tree.

Below, we are observing an empty object that will eventuality become a tree of nested objects.

```javascript
let obj = {};
Reflex.observe(obj, (newValues, oldValues, event) => {
    console.log(newValues, oldValues, event);
}, {observeDown:true});
```

We begin by nesting a child object under the root object. As normal, this assignment operation will fire our observer.

```javascript
Reflex.set(obj, ‘preferences’, {});
```

Here are the details of what our observer receives.

```text
// newValues and oldValues
newValues                {preferences: <reference to the received object>}
oldValues                {preferences: undefined}
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.created            [‘preferences’]
event.data                 {preferences: <reference to the received object>}
event._data             {preferences: undefined}
```

Now we will modify the tree at level 2 and watch the event bubble up to the observer at the root level.

```javascript
Reflex.set(obj.preferences, ‘fruit’, ‘apple’);
```

Here are the details of what our root-level observer receives:

```text
// newValues and oldValues
newValues                    {preferences: <reference to the modified obj.preferences>}
oldValues                    {preferences: <reference to the modified obj.preferences>}
// event details
event.type                     ‘set’
event.target                 <reference to the root object>
event.fields                [‘preferences’]
event.data                     {preferences: <reference to the modified obj.preferences>}
event._data                 {preferences: <reference to the modified obj.preferences>}
```

Notice above that the received _oldValues_ parameter does not represent a copy of the subtree before the modification; for deep changes, _newValues_ and _oldValues_ will be containing references to the same values – the state of the subtree after the change. In the same way, the received `event._data` and `event.data` will be containing references to the same values. The regular new/old value attributes of a change for deep changes can, however, be obtained from the following event properties.

```text
event.originatingData         {‘preferences.fruit’: ‘apple’}
event._originatingData         {‘preferences.fruit’: undefined}
```

And here are other information about the origin of the event:

```javascript
event.originatingTarget     <reference to obj.preferences>
event.originatingFields         [‘preferences.fruit’]
event.originatingCreated    [‘fruit’]
```

**Using a Path Expression**

When we observe a path on an object tree, changes that occur at any level along that path will bubble to the observer bound up the tree.

Below, we are observing a path on an empty object that will eventuality become a tree of nested objects.

```javascript
let obj = {};
Reflex.observe(obj, ‘preferences.fruit’, (newValue, oldValue, event) => {
    console.log(newValue, oldValue, event);
}, {observeDown:true});
```

As normal, we begin by nesting a child object under the root object. This assignment operation will fire our observer.

```javascript
Reflex.set(obj, ‘preferences’, {});
```

Here are the details of what our observer receives.

```text
// newValue and oldValue
newValue                undefined
oldValue                undefined
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.created            [‘preferences’]
event.data                 {preferences: <reference to the received object>}
event._data             {preferences: undefined}
```

Now, we will modify the tree at level 2 with properties.

We don’t expect this first assignment to fire our observer as this won’t be happening along the path we’re observing.

```javascript
Reflex.set(obj.preferences, ‘brand’, ‘apple’);
```

This next assignment fits right in and we should watch the event bubble up.

```javascript
Reflex.set(obj.preferences, ‘fruit’, ‘orange’);
```

Here are the details of what our root-level observer receives:

```text
// newValue and oldValue
newValue                ‘orange’
oldValue                undefined
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.data                 {preferences: <reference to the modified obj.preferences>}
event._data             {preferences: <reference to the modified obj.preferences>}
```

The new/old value attributes of this deep change can also be obtained from the following event properties.

```text
event.originatingData         {‘preferences.fruit’: ‘orange’}
event._originatingData         {‘preferences.fruit’: undefined}
```

And here are other information about the origin of the event:

```text
event.originatingTarget         <reference to obj.preferences>
event.originatingFields             [‘preferences.fruit’]
event.originatingCreated         [‘fruit’]
```

**Using Multiple Path Expressions**

When we observe multiple paths on an object tree, changes that occur at any level along any of the paths will bubble to the observer bound up the tree.

Below, we are observing two paths on an empty object that will eventuality become a tree of nested objects.

```javascript
let obj = {};
Reflex.observe(obj, [‘preferences.fruit’, ‘preferences.brand’], (newValuesList, oldValuesList, event) => {
    console.log(newValuesList, oldValuesList, event);
}, {observeDown:true});
```

Again, we begin by nesting a child object under the root object. This assignment operation will fire our observer. Also note that the nested child object is coming into the tree with an existing property.

```javascript
Reflex.set(obj, ‘preferences’, {brand:’windows’});
```

Here are the details of what our observer receives.

```javascript
// newValuesList and oldValuesList
newValuesList            [undefined, ’windows’]
oldValuesList            [undefined, undefined]
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.created            [‘preferences’]
event.data                 {preferences: <reference to the received object>}
event._data             {preferences: undefined}
```

Now, we will modify the tree at level 2 with another property.

```javascript
Reflex.set(obj.preferences, ‘fruit’, ‘orange’);
```

Here are the details of what our root-level observer receives:

```text
// newValuesList and oldValuesList
newValuesList            [‘orange’, ’windows’]
oldValuesList            [undefined, ’windows’]
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.data                 {preferences: <reference to the modified obj.preferences>}
event._data             {preferences: <reference to the modified obj.preferences>}
```

The new/old value attributes of this deep change can also be obtained from the following event properties.

```javascript
event.originatingData         {‘preferences.fruit’: ‘orange’}
event._originatingData         {‘preferences.fruit’: undefined}
```

And here are other information about the origin of the event:

```text
event.originatingTarget     <reference to obj.preferences>
event.originatingFields         [‘preferences.fruit’]
event.originatingCreated    [‘fruit’]
```

Next, we will modify the tree on an existing property at level 2.

```javascript
Reflex.set(obj.preferences, ‘brand’, ‘apple’);
```

Here are the details of what our root-level observer receives:

```text
// newValuesList and oldValuesList
newValuesList            [‘orange’, ‘apple’]
oldValuesList            [‘orange’, ’windows’]
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.data                 {preferences: <reference to the modified obj.preferences>}
event._data             {preferences: <reference to the modified obj.preferences>}
```

The new/old value attributes of this deep change can also be obtained from the following event properties.

```text
event.originatingData         {‘preferences.brand’: ‘apple’}
event._originatingData         {‘preferences. brand’: ’windows’}
```

And here are other information about the origin of the event:

```text
event.originatingTarget     <reference to obj.preferences>
event.originatingFields         [‘preferences.brand’]
event.originatingCreated    [‘brand’]
```

**Using a Wildcard Path Expression**

When we observe a wildcard path on an object tree, changes that occur at a descendant node that fulfills the wildcard path will bubble to the observer bound up the tree.

Below, we are observing the wildcard path on an empty object that will eventuality become a tree of nested objects. Notice that our path is a 3-level path with the second level being the wildcard.

```javascript
let obj = {};
Reflex.observe(obj, ‘preferences..fruit’, (newValue, oldValue, event) => {
    console.log(newValue, oldValue, event);
});
```

As usual, we begin by nesting a child object under the root object. This assignment operation will, however, NOT fire our observer as the path to this change \(‘preferences’\) won’t be fulfilling our wildcard path.

```javascript
Reflex.set(obj, ‘preferences’, {});
```

Now, we will extend the tree at level 2 with another object. Notice now that this assignment operation will fire our observer as the path to this change \(‘preferences.favourites’\) will have answered all missing slots in our wildcard path.

```javascript
Reflex.set(obj.preferences, ‘favourites’, {});
```

Here are the details of what our root-level observer receives:

```text
// newValue and oldValue
newValue                undefined
oldValue                undefined
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.data                 {preferences: <reference to the modified obj.preferences>}
event._data             {preferences: <reference to the modified obj.preferences>}
```

The new/old value attributes of this deep change can also be obtained from the following event properties.

```text
event.originatingData         {‘preferences.favourites’: <reference to obj.preferences.favourites>}
event._originatingData         {‘preferences.favourites’: undefined}
```

And here are other information about the origin of the event:

```text
event.originatingTarget     <reference to obj.preferences >
event.originatingFields         [‘preferences.favourites’]
event.originatingCreated    [‘favourites’]
```

Moving forward, we will set a property on the tree at level 3. This time, our observer should also be triggered as the event will be firing at a path \(preferences.favourites.fruit\) that fulfills our wildcard path!

```javascript
Reflex.set(obj.preferences.favourites, ‘fruit’, ‘mango’);
```

Here are the details of what our root-level observer receives:

```text
// newValue and oldValue
newValue                ‘mango’
oldValue                undefined
// event details
event.type                 ‘set’
event.target             <reference to the root object>
event.fields            [‘preferences’]
event.data                 {preferences: <reference to the modified obj.preferences>}
event._data             {preferences: <reference to the modified obj.preferences>}
```

The new/old value attributes of this deep change can also be obtained from the following event properties.

```text
event.originatingData         {‘preferences.favourites.fruit’: ‘mango’}
event._originatingData         {‘preferences.favourites.fruit’: undefined}
```

And here are other information about the origin of the event:

```text
event.originatingTarget     <reference to obj.preferences.favourites>
event.originatingFields         [‘preferences.favourites.fruit’]
event.originatingCreated    [‘fruit’]
```

### Reflex.unobserve

This method is used to unbind observers previously bound with Reflex.observe.

#### Syntax

```javascript
// Unbind all observers bound to the following property name
// regardless of the handler function
Reflex.unobserve(object, propertyName);

// Unbind the observer bound with the following handler function
Reflex.unobserve(object, propertyName, originalHandler);

// Unbind the observer bound with the following handler function and tag 
Reflex.unobserve(object, propertyName, originalHandler, {tag:originalReference});

// Unbind the observer bound with the following handler function and reflex type 
Reflex.unobserve(object, propertyName, originalHandler, {type:’set’});

// Unbind all observers bound with the following tag 
// regardless of the handler function
Reflex.unobserve(object, propertyName, null, {tag:originalReference});
```

Above, object can be any object or array.

#### Return Value

_Undefined_

### Reflex.trap

This method is used to intercept the mutations and queries performed on an object or array with custom handlers. Requests like _set_, _delete_, _get_ and _has_ are trapped and forwarded to these custom handler that may be designed to fulfill them in a special way. This comes in useful in implementing memorized properties, computed properties, or properties with remote values.

#### Syntax

```javascript
// Trap all operations or queries
Reflex.trap(object, handler[, params = {}]);
```

#### Basic Usage

```javascript
let obj = {name: ‘reflex’,};
let getVersionNumberRemotely = () => ‘1.0.0’;
```

Below, we’re trapping the “get” request and skipping all other requests using the next\(\) function we receive in our handler. This trap will “lazy-load” the object’s version value.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    if (event.type === ‘get’) {
        let requestedKey = event.query;
        if (requestedKey === ‘version’ && !(requestedKey in obj)) {
            obj[requestedKey]; =  getVersionNumberRemotely();
        }
        return obj[requestedKey];
    }
    return next();
});
```

Now, let’s see what we get for each property we access.

```javascript
console.log(Reflex.get(obj, ‘name’)); // ‘reflex’
console.log(Reflex.get(obj, ‘version’)); // ‘1.0.0’
```

In another case, we’re trapping a “set” operation to validate the incoming value for a specific property. We’re using the params.type to constrain the trap to just the “set” type.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    let requestedKey = event.query;
    let requestedValue = event.value;
    if (requestedKey === ‘url’ && !requestedValue.startsWith(‘http’)) {
        throw new Error(‘The url property only accepts a valid URL!’);
    }
    obj[requestedKey] = requestedValue;
    // We return true here
    // and it’s always good to still call next() with a return a value
    return next(true);
}, {type:’set’});
```

Now, let’s attempt setting different URLs on our object. Remember that Reflex.set will also trigger observers that may be bound to the object.

```javascript
console.log(Reflex.set(obj, ‘url’, ‘https://example.com’)); // true
console.log(Reflex.set(obj, ‘url’, ‘example.com’)); // Fatal Error
```

#### Tagging a Trap

The params.tag parameter can be used to tag a trap. This tag isn’t used specially except for identifying the trap. A tag can be anything: _string_, _number_, _object_, etc.

```javascript
Reflex.trap(object, handler, {tag:’#tag’});
```

#### Setting Multiple Traps

Multiple traps can rightly be set on an object. Each trap called will have the decision to call the next. A trap is called with the return value of the previous trap \(or _undefined_ where there is no previous trap\) and a reference to the next trap \(or a reference to the default handler where there is no next trap\).

Below, we set an additional trap to handle setting the url property. But this time, we wouldn’t bother if the previous trap has handled this.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    If (received === true) {
        console.log(‘A previous handler has handled this!’);
        return next(true);
    }
    // We could do the work here
    // or simply leave it to the default property setter
    return next();
}, {type:’set’});
```

#### Return Value

Reflex.trap returns a Trap object that acts as a reference to the trap just bound. The Trap object’s methods could be useful in other ways.

```javascript
let trapObject = Reflex.trap(object, handler);
// Synthetically fire the trap
trapObject.fire(eventInstance);

// Disconnect the trap
trapObject.disconnect();
```

### Reflex.untrap

This method is used to unbind traps previously bound with Reflex.trap.

#### Syntax

```javascript
// Unbind all traps
// regardless of the handler function
Reflex.untrap(object);

// Unbind the trap bound with the following handler function
Reflex.untrap(object, originalHandler);

// Unbind the trap bound with the following handler function and tag 
Reflex.untrap(object, originalHandler, {tag:originalReference});

// Unbind the trap bound with the following handler function and reflex type 
Reflex.untrap(object, originalHandler, {type:’set’});

// Unbind all traps bound with the following tag 
// regardless of the handler function
Reflex.untrap(object, null, {tag:originalReference});
```

Above, object can be any object or array.

#### Return Value

_undefined_

### Reflex.on

This method works for an event-only communication between listeners and event propagators over an object. Used in conjunction with this method is the Reflex.trigger method.

#### Syntax

```javascript
// Bind an event listener
Reflex.on(object, eventName, handler[, params = {}]);
```

Above, object can be any object or array.

#### Usage

```javascript
let obj = {};
Reflex.on(obj, ‘ready’, event => {
    console.log(event.type, event.details);
});
```

Now, we use the `Reflex.trigger` method to fire an event.

```javascript
Reflex.trigger(obj, ‘ready’);
Reflex.trigger(obj, ‘ready’), {details:{time:0}});
```

#### Tagging a Listener

The `params.tag` parameter can be used to tag a listener. This tag isn’t used specially except for identifying the listener. A tag can be anything: _string_, _number_, _object_, etc

```javascript
Reflex.on(object, eventName, handler, {tag:’#tag’});
```

#### Return Value

`Reflex.on` returns a _Listener_ object that acts as a reference to the listener just bound. The _Listener_ object’s methods could be useful in other ways.

```javascript
let listenerObject = Reflex.on(object, eventName, handler);
// Synthetically trigger the listener
listenerObject.fire(eventInstance);

// Disconnect the listener
listenerObject.disconnect();
```

### Reflex.off

This method is used to unbind listeners previously bound with Reflex.on.

#### Syntax

```javascript
// Unbind all listeners bound to the following event name
// regardless of the event handler
Reflex.off(object, eventName);

// Unbind the listener bound with the following event handler  
Reflex.off(object, eventName, originalHandler);

// Unbind the listener bound with the following event handler and tag 
Reflex.off(object, eventName, originalHandler, {tag:originalReference});

// Unbind the listener bound with the following tag 
// regardless of the event handler
Reflex.off(object, eventName, null, {tag:originalReference});
```

Above, object can be any object or array.

#### Return Value

_undefined_

### Reflex.trigger

This method is used to trigger event handlers bound with Reflex.on.

#### Syntax

```javascript
// Trigger an event
Reflex.trigger(object, eventName[, data = {}]);
```

#### Return Value

`Reflex.trigger` returns the fired event object. This event object could now be inspected about the disposition of its handlers, and this could inform our next action.

```javascript
let event = Reflex.trigger(object, eventName);
```

Inspect the event to see the disposition of the fired listeners.

```javascript
if (event.defaultPrevented) {
    // event.preventDefault() has been called by a handler
    // Or a handler returned false
} else if (event.propagationStopped) {
    // event.stopPropagation() has been called by a handler
    // Or a handler returned false
} else if (event.promises) {
    // event.promise() has been called by a handler
    // Or a handler returned a promise
    event.promises.then(() => {
    // When all promises resolve
    }).catch(() => {
    // When any of the promises fail
    });
}
```

### Reflex.set

This method is used to set the value of an object’s property. It corresponds to the JavaScript’s built-in `Reflect.set` function, which in itself is the programmatic alternative to the assignment expression – `a = b`.

`Reflex.set` brings the added benefit of triggering _observer_ and _trap_ reflexes.

#### Syntax

```javascript
// Set or modify a specific property
Reflex.set(object, propertyName, value);

// Set or modify a list of properties with the same value
Reflex.set(object, propertyNames, sharedValue);

// Perform multiple key/value assignments
Reflex.set(object, keyValueMap);
```

Above, object represents an object or array; and each of the assignment operation will notify any observers that may have been bound to the object.

#### Assigning On a Specific Property

```javascript
// On an object
Reflex.set(obj, ‘fruit’, ‘orange’);
// On an array
Reflex.set(arr, 0, ‘orange’);
```

#### Assigning On a List of Properties

```javascript
// On an object
Reflex.set(obj, [‘fruit’, ‘brand’], ‘apple’);
// On an array
Reflex.set(arr, [0, 3], ‘apple’);
```

#### Multiple Key/Value Assignment

```javascript
// On an object
Reflex.set(obj, {
    fruit:’apple’,
    brand:‘apple’
});

// On an array
// Provide key/value as an object
Reflex.set(arr, {
    0:’apple’,
    3:‘apple’
});
```

#### Return Value

`Reflex.set` by default returns _true_ or _false_ for an assignment operation. This corresponds to `Reflect.set`’s return value.

```javascript
let assignment = Reflex.set(object, propertyName, value);
console.log(assignment === true);
```

It is also possible to obtain the fired event object as the return value. This event object could now be inspected about the disposition of its observers, and this could inform our next action.

To obtain the event object, pass _true_ after the list of arguments to `Reflex.set`.

```javascript
let event = Reflex.set(object, propertyName, value, true/*returnEvent*/);
let event = Reflex.set(object, propertyNames, value, true/*returnEvent*/);
let event = Reflex.set(object, keyValueMap, true/*returnEvent*/);
```

Inspect the event to see the disposition of the fired observers.

```javascript
if (event.defaultPrevented) {
    // event.preventDefault() has been called by an observer
    // Or an observer returned false
} else if (event.propagationStopped) {
    // event.stopPropagation() has been called by an observer
    // Or an observer returned false
} else if (event.promises) {
    // event.promise() has been called by an observer
    // Or an observer returned a promise
    event.promises.then(() => {
    // When all promises resolve
    }).catch(() => {
    // When any of the promises fail
    });
}
```

#### Usage as a Trap’s “set” Handler

Returning the _Boolean_ type by default makes `Reflex.set` perfect as the “set” handler for JavaScript traps. A general usecase is with Proxy traps.

```javascript
let _obj = new Proxy(obj, {set: Reflex.set});
let _arr = new Proxy(arr, {set: Reflex.set});
```

Assignment operations will now be forwarded to Reflex.set and reflex actions will continue to fire as normal.

```javascript
_obj.fruit = ‘apple’;
_arr[2] = ‘Item 3’;
```

Things get even more interesting with arrays as Reflex.set is also able to detect changes made by array prototype methods.

```javascript
_arr.push(‘apple’);
_arr.pop();
_arr.unshift(‘orange’);
_arr.splice(0);
```

Another trap usecase is with JSEN traps. Here’s how we could drive reflex actions as we evaluate a JSEN “assignment” expression:

```javascript
let expr = ‘fruit = “pineapple”’;
let exprParse = JSEN.parse(expr);

// Evaluate the expression in the context of “obj”, with Reflex.set as the trap’s “set” handler.
exprParse.eval(obj, {set: Reflex.set});
```

The above will fire observers bound to obj and obj.fruit will be pineapple.

#### Usage with Property Setters

It is possible to implement _property setters_ that use `Reflex.set` behind the scene. This gives us the benefit of using JavaScript’s assignment syntax while still driving reflex actions.

This is automatically done by the Reflex.init support function.

```javascript
// Virtualize a property or multiple properties
Reflex.init(obj, ‘fruit’);
Reflex.init(obj, [‘fruit’, ‘brand’]);

// Now we can do without Reflex.set
obj.fruit = ‘apple’;
obj.brand = ‘apple’;
```

While we could follow the pattern above for arrays, we could _init_ an array’s prototype methods instead. The specific keys modified after calling these methods will be announced in reflex actions.

```javascript
// Virtualize the arr.push() and arr.splice() methods
Reflex.init(arr, [‘push’, ‘splice’]);

// Now we can do without Reflex.set
arr.push(‘Item 1’);
arr.push(‘Item 2’);
arr.splice(1);
```

#### Trapping Reflex.set

As seen in the `Reflex.trap` section, it is possible to intercept calls to `Reflex.set`. When a “set” operation triggers a trap, the trap handler will receive an event object containing the target property name and the assignable value. When the “set” operation is of multiple key/value assignments, the trap gets fired for each pair while also providing a hint about the total list of properties in the batch.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    // The target property name
    console.log(event.key);
    // The assignable value
    console.log(event.value);
    // The total list of properties in the batch
    console.log(event.related);
}, {type:’set’});

Reflex.set(obj, {fruit: ‘orange’, brand:‘apple’});
```

The above should trigger our trap twice with `event.related` being `[‘fruit’, ‘brand’]`.

The trap is expected to return _true_ if the operation is successful and _false_ otherwise.

### Reflex.deleteProperty

This method is used to delete a property from an object. It corresponds to the JavaScript’s built-in `Reflect.deleteProperty` function, which in itself is the programmatic alternative to the delete keyword – `delete a.b`.

`Reflex.deleteProperty` brings the added benefit of triggering _observer_ and _trap_ reflexes.

The `Reflex.del` function is an alias of this function and could be used interchangeably.

#### Syntax

```javascript
// Delete a specific property
Reflex.deleteProperty(object, propertyName);

// Delete a list of properties
Reflex.deleteProperty(object, propertyNames);
```

Above, object represents an object or array; and each of the delete operation will notify any observers that may have been bound to the object.

#### Deleting a Specific Property

```javascript
// On an object
Reflex.deleteProperty(obj, ‘fruit’);
// On an array
Reflex.deleteProperty(arr, 0);
```

#### Deleting a List of Properties

```javascript
// On an object
Reflex.deleteProperty(obj, [‘fruit’, ‘brand’]);
// On an array
Reflex.deleteProperty(arr, [0, 3]);
```

#### Return Value

`Reflex.deleteProperty` by default returns _true_ or _false_ for a delete operation. This corresponds to `Reflect.deleteProperty`’s return value.

```javascript
let deletion = Reflex.deleteProperty(object, propertyName);
console.log(deletion === true);
```

It is also possible to obtain the fired event object as the return value. This event object could now be inspected about the disposition of its observers, and this could inform our next action.

To obtain the event object, pass _true_ after the list of arguments to `Reflex.deleteProperty`.

```javascript
let event = Reflex.deleteProperty(object, propertyName, true/*returnEvent*/);
let event = Reflex.deleteProperty(object, propertyNames, true/*returnEvent*/);
```

Inspect the event to see the disposition of the fired observers.

```javascript
if (event.defaultPrevented) {
    // event.preventDefault() has been called by an observer
    // Or an observer returned false
} else if (event.propagationStopped) {
    // event.stopPropagation() has been called by an observer
    // Or an observer returned false
} else if (event.promises) {
    // event.promise() has been called by an observer
    // Or an observer returned a promise
    event.promises.then(() => {
    // When all promises resolve
    }).catch(() => {
    // When any of the promises fail
    });
}
```

#### Usage as a Trap’s “deleteProperty” Handler

Returning the _Boolean_ type by default makes `Reflex.deleteProperty` perfect as the “deleteProperty” handler for JavaScript traps. A general usecase is with Proxy traps.

```javascript
let _obj = new Proxy(obj, {deleteProperty: Reflex.deleteProperty});
let _arr = new Proxy(arr, {deleteProperty: Reflex.deleteProperty});
```

Delete operations will now be forwarded to `Reflex.deleteProperty` and reflex actions will continue to fire as normal.

```javascript
delete _obj.fruit;
delete _arr[2];
```

Another trap usecase is with JSEN traps. Here’s how that could drive reflex actions as we evaluate a JSEN “delete” expression:

```javascript
let expr = ‘delete fruit’;
let exprParse = JSEN.parse(expr);

// Evaluate the expression in the context of “obj” and with Reflex.deleteProperty as the trap’s “deleteProperty” handler.
exprParse.eval(obj, {deleteProperty: Reflex.deleteProperty});
```

#### Trapping Reflex.deleteProperty

It is possible to intercept calls to `Reflex.deleteProperty`. When a “del” operation triggers a trap, the trap handler will receive an event object containing the target property name. When the “del” operation is of multiple property deletions, the trap gets fired for each property while also providing a hint about the total list of properties in the batch.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    // The target property name
    console.log(event.query);
    // The total list of properties in the batch
    console.log(event.related);
}, {type:’set’});

Reflex.del(obj, [‘fruit’, ‘brand’]);
```

The above should trigger our trap twice with `event.related` being `[‘fruit’, ‘brand’]`.

The trap is expected to return _true_ if the operation is successful and _false_ otherwise.

### Reflex.defineProperty

This method is used to define a property on an object or modifies an existing property. It corresponds to the JavaScript’s built-in `Reflect.defineProperty` function.

`Reflex.defineProperty` brings the added benefit of driving _observer_ and _trap_ reflexes.

The `Reflex.def` function is an alias of this function and could be used interchangeably.

#### Syntax

```javascript
// Define a property
Reflex.defineProperty(object, propertyName, propertyDescriptor);
```

The operation will work exactly like `Reflect.defineProperty` while also notifying any observers that may have been bound to the object.

#### Defining a Property

```javascript
// On an object
Reflex.defineProperty(obj, ‘fruit’, {value:’orange’});
```

The above will trigger observers just like a regular “set” operation.

#### Return Value

`Reflex.defineProperty` by default returns _true_ or _false_. This corresponds to `Reflect.defineProperty`’s return value.

```javascript
let definition = Reflex.defineProperty(object, propertyName, propertyDescriptor);
console.log(definition === true);
```

It is also possible to obtain the fired event object as the return value. This event object could now be inspected about the disposition of its observers, and this could inform our next action.

To obtain the event object, pass _true_ after the list of arguments to `Reflex.defineProperty`.

```javascript
let event = Reflex.defineProperty(object, propertyName, propertyDescriptor, true/*returnEvent*/);
```

Inspect the event to see the disposition of the fired observers.

```javascript
if (event.defaultPrevented) {
    // event.preventDefault() has been called by an observer
    // Or an observer returned false
} else if (event.propagationStopped) {
    // event.stopPropagation() has been called by an observer
    // Or an observer returned false
} else if (event.promises) {
    // event.promise() has been called by an observer
    // Or an observer returned a promise
    event.promises.then(() => {
    // When all promises resolve
    }).catch(() => {
    // When any of the promises fail
    });
}
```

#### Usage as a Trap’s “defineProperty” Handler

Returning the _Boolean_ type by default makes `Reflex.defineProperty` perfect as the “defineProperty” handler for JavaScript traps. A general usecase is with Proxy traps.

```javascript
let _obj = new Proxy(obj, {defineProperty: Reflex.defineProperty});
```

Property definitions will now be forwarded to `Reflex.defineProperty` and reflex actions will continue to fire as normal.

```javascript
Reflect.defineProperty(_obj, ‘fruit’, {value:’apple’});
```

#### Trapping Reflex.defineProperty

It is possible to intercept calls to `Reflex.defineProperty`. When a “def” operation triggers a trap, the trap handler will receive an event object containing the target property name and the property’s _descriptor_ object.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    // The target property name
    console.log(event.query);
    // The property descriptor object
    console.log(event.descriptor);
}, {type:’def’});

Reflex.defineProperty(obj, ‘fruit’, {value:’orange’});
```

The above should trigger our trap.

The trap is expected to return _true_ if the operation is successful and _false_ otherwise.

### Reflex.get

This method is used to read a property. It corresponds to the JavaScript’s built-in `Reflect.get` function, which in itself is the programmatic alternative to the syntax for property access – `a.b; a[‘b’]`.

`Reflex.get` brings the added benefit of driving trap reflexes.

#### Syntax

```javascript
// Read a specific property.
// The return value will be the value of the property
var value = Reflex.get(object, propertyName);

// Read a list of properties
// The return value will be a key/value map of the listed properties
var values = Reflex.get(object, propertyNames);
```

Above, object represents an object or array.

#### Trapping Reflex.get

As seen in the `Reflex.trap` section, it is possible to intercept calls to `Reflex.get`. When a “get” query triggers a trap, the trap handler will receive an event object containing the property name or property names.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    // The target property name or property names
    // This could be a string, number, or array
    console.log(event.query);
}, {type:’get’});

let preferences  = Reflex.get(obj, [‘fruit’, ‘brand’]);
```

The above should trigger our trap once and preferences should look like `{fruit:<val>, brand:<val>}`.

#### Usage as a Trap’s “get” Handler

Just like `Reflect.get`, `Reflex.get` can be used as the “get” handler for JavaScript traps. A general usecase is with Proxy traps.

```javascript
let _obj = new Proxy(obj, {get: Reflex.get});
let __arr = new Proxy(arr, {get: Reflex.get});
```

Get queries will now be forwarded to `Reflex.get` and trap reflexes will continue to fire as normal.

```javascript
let fruit = _obj.fruit;
let fruit = _arr[2];
```

Another trap usecase is with JSEN traps. Here’s how that could drive reflex actions as we evaluate a JSEN “get” expression:

```javascript
let expr = ‘fruit’;
let exprParse = JSEN.parse(expr);

// Evaluate the expression in the context of “obj” and with Reflex.get as the trap’s “get” handler.
exprParse.eval(obj, {get: Reflex.get});
```

#### Usage with Property Getters

It is possible to implement _property getters_ that use `Reflex.get` behind the scene. This gives us the benefit of using JavaScript’s accessor syntax while still driving reflex actions.

This is automatically done by the `Reflex.init` support function.

```javascript
// Virtualize a property or multiple properties
Reflex.init(obj, ‘fruit’);
Reflex.init(obj, [‘fruit’, ‘brand’]);

// Now we can do without Reflex.get while still driving trap reflexes
console.log(obj.fruit);
```

### Reflex.has

This method is used to test property presence. It corresponds to the JavaScript’s built-in `Reflect.has` function, which in itself is the programmatic alternative to the in operator – `a in b`.

`Reflex.has` brings the added benefit of driving trap reflexes.

#### Syntax

```javascript
// Test the presence of a property.
let exists = Reflex.has(object, propertyName);
```

Above, object represents an object or array.

#### Trapping Reflex.has

It is possible to intercept calls to `Reflex.has`. When a “has” query triggers a trap, the trap handler will receive an event object containing the property name.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    // The property name
    console.log(event.query);
}, {type:’has’});

let exists  = Reflex.has(obj, ‘fruit’);
```

The above should trigger our trap. The trap is expected to return a _Boolean true_ or _false_.

#### Usage as a Trap’s “has” Handler

Just like `Reflect.set`, `Reflex.has` can be used as the “has” handler for JavaScript traps. A general usecase is with Proxy traps.

```javascript
let _obj = new Proxy(obj, {has: Reflex.has});
let __arr = new Proxy(arr, {has: Reflex.has});
```

Presence tests will now be forwarded to `Reflex.has` and trap reflexes will be fired.

```javascript
console.log(‘fruit’ in obj);
console.log(2 in arr);
```

Another trap usecase is with JSEN traps. Here’s how that could drive reflex actions as we evaluate a JSEN “get” expression:

```javascript
let expr = ‘”fruit” in preferences’;
let exprParse = JSEN.parse(expr);

// Evaluate the expression in the context of an object and with Reflex.has as the trap’s “has” handler.
exprParse.eval({preferences: {brand:’apple’}}, {has: Reflex.has});
```

### Reflex.ownKeys

This method is used to get an object’s list of direct properties. It corresponds to the JavaScript’s built-in `Reflect.ownKeys` function.

`Reflex.ownKeys` brings the added benefit of driving _trap_ reflexes.

`The Reflex.keys` function works in a similar way but corresponds more with the build-in Object.keys.

#### Syntax

```javascript
// Show all keys.
let keys = Reflex.ownKeys(object);
```

Above, object represents an object or array.

#### Trapping Reflex.ownKeys

It is possible to intercept calls to `Reflex.ownKeys` \(as well as calls to `Reflex.keys`\). Traps will be triggered by “ownKeys” or “keys” queries.

```javascript
Reflex.trap(obj, (event, recieved, next) => {
    // The query type
    console.log(event.type);
});

let keys  = Reflex.keys(obj);
```

The above should trigger our trap. The trap is expected to return an array.

#### Usage as a Trap’s “ownKeys” Handler

Just like `Reflect.ownKeys`, `Reflex.ownKeys` can be used as the “ownKeys” handler for JavaScript traps. A general usecase is with Proxy traps.

```javascript
let _obj = new Proxy(obj, {ownKeys: Reflex.ownKeys});
```

“ownKeys” requests will now be forwarded to `Reflex.ownKeys` and trap reflexes will be fired.

```javascript
console.log(Reflect.ownKeys(obj));
```

## Support Functions

### Reflex.init

This function is used to implement property _setters_ and _getters_ that use `Reflex.set` and `Reflex.get` respectively behind the scene. This gives us the benefit of using JavaScript’s assignment and accessor syntax while still driving reflex actions.

#### Syntax

```javascript
// Init a single property
Reflex.init(object, propertyName);

// Init multiple properties
Reflex.init(object, propertyNames);
```

#### Usage

```javascript
// The object
let obj = {};

// We observe the ‘preferences’ property
Reflex.observe(obj, ‘preferences’, newValue => {
    console.log(newValue);
});

// Now we virtualize this property
Reflex.init(obj, ‘preferences’);

// We use the property and watch our console.
obj.preferences = {};
```

### Reflex.link

This function is used to link a child object to a parent object so that changes effected on child can bubble up to observers at parent level. This function becomes useful in adding “reflexivity” between objects that already maintain a _parent-child_ relationship. For child objects set using the `Reflex.set` function, this linking is automatically done.

#### Syntax

```javascript
Reflex.link(object, offset, childObject);
```

#### Usage

```javascript
// The relationship formed outside of Reflex
let parent = {};
let child = {};
parent.preferences = child;

// The Reflexive linking
Reflex.link(parent, ‘preferences’, child);
```

With the linking above, changes made in child should be observable from parent.

### Reflex.unlink

This function is used to unlink objects previously linked with `Reflex.link`.

**Syntax**

```javascript
Reflex.unlink(object, offset, childObject);
```

### Reflex.build

This function recursively links objects in an object tree for Reflex actions. This is the self-driven version of `Reflex.link`.

#### Syntax

```javascript
Reflex.build(object[, init = false]);
```

Here, object can be any object or array. The init parameter specifies whether to also initialize properties down the tree using `Reflex.init`.

#### Usage

```javascript
// The object tree
let obj = {
    preferences: {
        favourites: {},
    },
};

// We observe the tree
Reflex.observe(obj, (newValues, oldValues, event) => {
    console.log(newValues, oldValues, event);
}, {observeDown:true});

// Now we build “reflex” into the tree
Reflex.build(obj);

// We modify the tree at any level and watch our console.
obj.preferences.favourites.fruit = ‘apple’;
```

### Reflex.transaction

This function provides a _Reflex-aware_ context under which to execute code that could potentially mutate an observed object in some unpredictable way. Under this context, all mutations made to an observed will be detected and observes that may be bound to the object will be triggered.

#### Syntax

```javascript
// Establish a transaction on multiple objects
Reflex.transaction([object1, object, …], callback[, keys = [][, returnEvent = false]]);
```

Above, the list of objects can be a mix of any object or array. _callback_ is the function that executes the unpredictable code. _keys_ is an optional list of specific keys to watch out for. _returnEvent_ specifies whether the “transaction” event object made for the transaction should be returned; otherwise an array of changed keys is returned.

#### Usage

```javascript
// The observed object/array
let arr = [];
Reflex.observe(arr, [0, 2], newValues => {
    console.log(newValues);
});

// The transaction
Reflex.transaction([arr], () => {
    arr.push(‘one’);
    arr.push(‘two’);
    arr.push(‘three’);
    arr.push(‘’four’);
    arr.shift();
});
```

The above transaction should be reported in the console as `[‘two’, ‘four’]`.

Notice that changes are detected and observers fired after the transaction code runs. These changes are detected by comparing the state of the object with its state before the transaction. Intermediate changes, therefore, do not get caught, and “set” and “get” traps that may have been bound to the object don’t get to be fired.

