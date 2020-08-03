# PlayUI API

## Individual Functions Documentation

PlayUI is designed as a collection of standalone functions organized in categories.

* [The DOM](/play-ui/v002/api/dom/README.md) - DOM manipulation utilities.
* [CSS](/play-ui/v002/api/css/README.md) - CSS-processing utilities.
* [Animation](/play-ui/v002/api/ani/README.md) - Animation utilities.
* [Event](/play-ui/v002/api/evt/README.md) - Utilities for binding events and gestures.
* [The UI](/play-ui/v002/api/ui/README.md) - Utilities for manipulating the rendered UI.

## Constructible Build Documentation

PlayUI is also implemented as a *constructible function* (like the jQuery function). An instance can be created by using the `new` operator or by calling the function statically.

```js
// Import PlayUI
import PlayUI from '@web-native-js/play-ui';
// Or, if loaded via a script tag...
const PlayUI = window.WebNative.PlayUI;

// Create an instance with the new operator
let instance = new PlayUI(element);
// Or by calling the function statically
let instance = PlayUI(element);
```

And we can delegate PlayUI to the shorter `$` variable.

```js
// Import PlayUI to the $ variable locally
import $ from '@web-native-js/play-ui';
// Or, if loaded via a script tag...
const $ = window.WebNative.PlayUI;

// Create an instance as before...
let instance = $(element); // or new $(element)
```

Instances of PlayUI are built from the same PlayUI utility functions above. Each instance method thus has the same function definition as its corresponding utility function.

The `.htmlSync()` instance method, for example, is based on the utility function at `./src/dom/htmlSync.js`.

```js
// The .htmlSync() instance method
let instance = $(element).htmlSync(content);

// --------------------------

// The actual standalone htmlSync() function
import htmlSync from './src/dom/htmlSync.js';
let element = htmlSync(element, content);
```

Notice that where the utility function would return the same subject element as recieved, the instance method would return the PlayUI instance itself for easy method chaining.

Also, where the returned *promise* of an *async* utility function resolves to the same subject element as recieved, the returned *promise* of the corresponding instance method would resolve to the PlayUI instance itself.

```js
// The .htmlAsync() instance method
$(element).htmlAsync(content).then(instance => {
    // instance.somethingElse()
});
// Or within an async function
let instance = await $(element).htmlAsync(content);

// --------------------------

// The actual standalone htmlAsync() function
import htmlAsync from './src/dom/htmlAsync.js';
htmlAsync(element, content).then(element => {
    // somethingElse(element)
});
// Or within an async function
let element = await htmlAsync(element, content);
```

### DOM Manipulation Methods

DOM manipulation methods. The most of these methods come in a *sync* and *async* flavour, and a third unsiffixed flavour that is an alias to the *async* method.

#### `instance.htmlAsync()`

This method *asynchronously* sets or gets the HTML content for the current matched element. It uses the [`dom/htmlAsync()`](/play-ui/v002/api/dom/htmlasync.md) function.

This method _asynchronously_ sets or gets the HTML content for the current matched element. It uses the [`dom/htmlAsync()`](dom/htmlasync.md) function.

```javascript
// Set the HTML content, do something else afterward
$('#el').htmlAsync('Hello player!').then(instance => {
    // Do something else
});

// Get the HTML content, log it to the console when available
$('#el').htmlAsync().then(content => {
    console.log(content);
});
```

See [`dom/htmlAsync()`](/play-ui/v002/api/dom/htmlasync.md) for details.

#### `instance.htmlSync()`

This method *synchronously* sets or gets the HTML content for the current matched element. It uses the [`dom/htmlSync()`](/play-ui/v002/api/dom/htmlsync.md) function.

This method _synchronously_ sets or gets the HTML content for the current matched element. It uses the [`dom/htmlSync()`](https://github.com/web-native/docs/tree/201e05f348ffbec3ef9e613f36713ffdc10badc9/play-ui/api/dom/htmlsync.md) function.

```javascript
// Set the HTML content
let instance = $('#el').htmlSync('Hello player!');

// Get the HTML content, log it to the console
let content = $('#el').htmlSync();
console.log(content);
```

See [`dom/htmlSync()`](/play-ui/v002/api/dom/htmlsync.md) for details.

#### `instance.html()`

This is an alias of the [`instance.htmlAsync()`](#instancehtmlasync) method.

#### `instance.textAsync()`

This method *asynchronously* sets or gets the text content for the current matched element. It uses the [`dom/textAsync()`](/play-ui/v002/api/dom/textasync.md) function.

This method _asynchronously_ sets or gets the text content for the current matched element. It uses the [`dom/textAsync()`](dom/textasync.md) function.

```javascript
// Set the text content, do something else afterward
$('#el').textAsync('Hello player!').then(instance => {
    // Do something else
});

// Get the text content, log it to the console when available
$('#el').textAsync().then(content => {
    console.log(content);
});
```

See [`dom/textAsync()`](/play-ui/v002/api/dom/textasync.md) for details.

#### `instance.textSync()`

This method *synchronously* sets or gets the text content for the current matched element. It uses the [`dom/textSync()`](/play-ui/v002/api/dom/textsync.md) function.

This method _synchronously_ sets or gets the text content for the current matched element. It uses the [`dom/textSync()`](https://github.com/web-native/docs/tree/201e05f348ffbec3ef9e613f36713ffdc10badc9/play-ui/api/dom/textsync.md) function.

```javascript
// Set the text content
let instance = $('#el').textSync('Hello player!');

// Get the text content, log it to the console
let content = $('#el').textSync();
console.log(content);
```

See [`dom/textSync()`](/play-ui/v002/api/dom/textsync.md) for details.

#### `instance.text()`

This is an alias of the [`instance.textAsync()`](#instancetextasync) method.

#### `instance.appendAsync()`

This method *asynchronously* appends text content to the current matched element. It uses the [`dom/appendAsync()`](/play-ui/v002/api/dom/appendasync.md) function.

This method _asynchronously_ appends text content to the current matched element. It uses the [`dom/appendAsync()`](dom/appendasync.md) function.

```javascript
// Append content, do something else afterward
$('#el').appendAsync('Hello player!').then(instance => {
    // Do something else
});
```

See [`dom/appendAsync()`](/play-ui/v002/api/dom/appendasync.md) for details.

#### `instance.appendSync()`

This method *synchronously* appends text content to the current matched element. It uses the [`dom/appendSync()`](/play-ui/v002/api/dom/appendsync.md) function.

This method _synchronously_ appends text content to the current matched element. It uses the [`dom/appendSync()`](https://github.com/web-native/docs/tree/201e05f348ffbec3ef9e613f36713ffdc10badc9/play-ui/api/dom/appendsync.md) function.

```javascript
// Append content
let instance = $('#el').appendSync('Hello player!');
```

See [`dom/appendSync()`](/play-ui/v002/api/dom/appendsync.md) for details.

#### `instance.append()`

This is an alias of the [`instance.appendAsync()`](#instanceappendasync) method.

#### `instance.prependAsync()`

This method *asynchronously* prepends text content to the current matched element. It uses the [`dom/prependAsync()`](/play-ui/v002/api/dom/prependasync.md) function.

This method _asynchronously_ prepends text content to the current matched element. It uses the [`dom/prependAsync()`](dom/prependasync.md) function.

```javascript
// Prepend content, do something else afterward
$('#el').prependAsync('Hello player!').then(instance => {
    // Do something else
});
```

See [`dom/prependAsync()`](/play-ui/v002/api/dom/prependasync.md) for details.

#### `instance.prependSync()`

This method *synchronously* prepends text content to the current matched element. It uses the [`dom/prependSync()`](/play-ui/v002/api/dom/prependsync.md) function.

This method _synchronously_ prepends text content to the current matched element. It uses the [`dom/prependSync()`](dom/prependsync.md) function.

```javascript
// Prepend content
let instance = $('#el').prependSync('Hello player!');
```

See [`dom/prependSync()`](/play-ui/v002/api/dom/prependsync.md) for details.

#### `instance.prepend()`

This is an alias of the [`instance.prependAsync()`](#instanceprependasync) method.

#### `instance.attrAsync()`

This method *asynchronously* sets or gets an attribute for the current matched element. It uses the [`dom/attrAsync()`](/play-ui/v002/api/dom/attrasync.md) function.

This method _asynchronously_ sets or gets an attribute for the current matched element. It uses the [`dom/attrAsync()`](dom/attrasync.md) function.

```javascript
// Set an attribute, do something else afterward
$('#el').attrAsync('name', 'value').then(instance => {
    // Do something else
});

// Get an attribute, log it to the console when available
$('#el').attrAsync('name').then(value => {
    console.log(value);
});
```

See [`dom/attrAsync()`](/play-ui/v002/api/dom/attrasync.md) for details.

#### `instance.attrSync()`

This method *synchronously* sets or gets an attribute for the current matched element. It uses the [`dom/attrSync()`](/play-ui/v002/api/dom/attrsync.md) function.

This method _synchronously_ sets or gets an attribute for the current matched element. It uses the [`dom/attrSync()`](dom/attrsync.md) function.

```javascript
// Set an attribute
let instance = $('#el').attrSync('name', 'value');

// Get an attribute, log it to the console
let value = $('#el').attrSync('name');
console.log(value);
```

See [`dom/attrSync()`](/play-ui/v002/api/dom/attrsync.md) for details.

#### `instance.attr()`

This is an alias of the [`instance.attrAsync()`](#instanceattrasync) method.

#### `instance.classAsync()`

This method is used to *asynchronously* set/unset the *class* attribute or add/remove entries on the *class* list for the current matched element. It uses the [`dom/classAsync()`](/play-ui/v002/api/dom/classasync.md) function.

This method is used to _asynchronously_ set/unset the _class_ attribute or add/remove entries on the _class_ list for the current matched element. It uses the [`dom/classAsync()`](dom/classasync.md) function.

```javascript
// Add a class, do something else afterward
$('#el').classAsync('class1', true).then(instance => {
    // Do something else
});

// Remove a class
$('#el').classAsync('class1', false).then(instance => {
    // Do something else
});
```

See [`dom/classAsync()`](/play-ui/v002/api/dom/classasync.md) for details.

#### `instance.classSync()`

This method is used to *synchronously* set/unset the *class* attribute or add/remove entries on the *class* list for the current matched element. It uses the [`dom/classSync()`](/play-ui/v002/api/dom/classsync.md) function.

This method is used to _synchronously_ set/unset the _class_ attribute or add/remove entries on the _class_ list for the current matched element. It uses the [`dom/classSync()`](dom/classsync.md) function.

```javascript
// Add a class
let instance = $('#el').classAsync('class1', true);

// Remove a class
let instance = $('#el').classAsync('class1', false);
```

See [`dom/classSync()`](/play-ui/v002/api/dom/classsync.md) for details.

#### `instance.class()`

This is an alias of the [`instance.classAsync()`](#instanceclassasync) method.

### CSS-Processing Methods

CSS-processing methods. A few of these methods come in a *sync* and *async* flavour, and a third unsiffixed flavour that is an alias to the *async* method.

#### `instance.cssAsync()`

This method *asynchronously* sets or gets CSS rules for the current matched element. It uses the [`css/cssAsync()`](/play-ui/v002/api/css/cssasync.md) function.

This method _asynchronously_ sets or gets CSS rules for the current matched element. It uses the [`css/cssAsync()`](css/cssasync.md) function.

```javascript
// Set a CSS rule, do something else afterward
$('#el').cssAsync('color', 'blue').then(instance => {
    // Do something else
});

// Get a CSS rule, log it to the console when available
$('#el').cssAsync('color').then(value => {
    console.log(value);
});
```

See [`css/cssAsync()`](/play-ui/v002/api/css/cssasync.md) for details.

#### `instance.cssSync()`

This method *synchronously* sets or gets CSS rules for the current matched element. It uses the [`css/cssSync()`](/play-ui/v002/api/css/csssync.md) function.

This method _synchronously_ sets or gets CSS rules for the current matched element. It uses the [`css/cssSync()`](css/csssync.md) function.

```javascript
// Set a CSS rule
let instance = $('#el').cssSync('color', 'blue');

// Get a CSS rule, log it to the console
let value = $('#el').cssSync('color');
console.log(value);
```

See [`css/cssSync()`](/play-ui/v002/api/css/csssync.md) for details.

#### `instance.css()`

This is an alias of the [`instance.cssAsync()`](#instancecssasync) method.

#### `instance.cssInline()`

This method returns one or more style properties from the current matched element's style attribute. It uses the [`css/readInline()`](/play-ui/v002/api/css/readinline.md) function.

This method returns one or more style properties from the current matched element's style attribute. It uses the [`css/readInline()`](css/readinline.md) function.

```javascript
// Read an inline CSS rule
let value = $('#el').cssInline('color');
console.log(value);
```

See [`css/readInline()`](/play-ui/v002/api/css/readinline.md) for details.

#### `instance.cssStylesheet()`

This method returns one or more style properties associated with the current matched element accross the document's stylesheets. It uses the [`css/readStylesheet()`](/play-ui/v002/api/css/readstylesheet.md) function.

This method returns one or more style properties associated with the current matched element accross the document's stylesheets. It uses the [`css/readStylesheet()`](css/readstylesheet.md) function.

```javascript
// Read a CSS rule from stylesheet
let value = $('#el').cssStylesheet('color');
console.log(value);
```

See [`css/readStylesheet()`](/play-ui/v002/api/css/readstylesheet.md) for details.

#### `instance.cssTransaction()`

This method establishes a CSS operation on the current matched element that can be rolledback safely. It uses the [`css/transaction()`](/play-ui/v002/api/css/transaction.md) function.

This method establishes a CSS operation on the current matched element that can be rolledback safely. It uses the [`css/transaction()`](css/transaction.md) function.

```javascript
// Obtaine a Transaction instance
let transaction = $('#el').cssTransaction('color');

// Create a savepoint - savepoint1
// We can rollback to this point later
transaction.save();

// Restyle the element
$('#el').css('color', 'green');

// Rollback to anypoint after some time
setTimeout(() => {
    // Rollback all the way to the element's initial state
    transaction.rollback(0);
}, 2000);
```

See [`css/transaction()`](/play-ui/v002/api/css/transaction.md) for details.

### Animation Methods

Methods for animation.

#### `instance.play()`

This method creates and play an animation on the current matched element. It uses the [`ani/play()`](/play-ui/v002/api/ani/play.md) function.

This method creates and play an animation on the current matched element. It uses the [`ani/play()`](ani/play.md) function.

```javascript
// Play fadeout
$('#el').play([{opacity:1}, {opacity:0}], {duration:600}).then(instance => {
    console.log('The end!');
});
```

See [`ani/play()`](/play-ui/v002/api/ani/play.md) for details.

### Event Methods

Methods for binding events and gestures.

#### `instance.on()`

This method binds an event/gesture handler to the current matched element. It uses the [`evt/on()`](/play-ui/v002/api/evt/on.md) function.

This method binds an event/gesture handler to the current matched element. It uses the [`evt/on()`](evt/on.md) function.

```javascript
// Bind a "doubletap" gesture handler
$('#el').on('doubletap', event => {
    alert('You doubletapped me!');
});
```

See [`evt/on()`](/play-ui/v002/api/evt/on.md) for details.

#### `instance.off()`

This method unbinds event/gesture handlers oreviously bound using the [`instance.on()`](#instanceon) method. It uses the [`evt/off()`](/play-ui/v002/api/evt/off.md) function.

This method unbinds event/gesture handlers oreviously bound using the [`instance.on()`](./#instanceon) method. It uses the [`evt/off()`](evt/off.md) function.

```javascript
// Unbind "doubletap" gesture handlers
$('#el').off('doubletap';
```

See [`evt/off()`](/play-ui/v002/api/evt/off.md) for details.

#### `instance.trigger()`

This method triggers event/gesture handlers oreviously bound using the [`instance.on()`](#instanceon) method. It uses the [`evt/trigger()`](/play-ui/v002/api/evt/trigger.md) function.

This method triggers event/gesture handlers oreviously bound using the [`instance.on()`](./#instanceon) method. It uses the [`evt/trigger()`](evt/trigger.md) function.

```javascript
// Unbind "doubletap" gesture handlers
$('#el').trigger('doubletap';
```

See [`evt/trigger()`](/play-ui/v002/api/evt/trigger.md) for details.

### Other Methods

Methods on cross-cutting concerns.

#### `instance.data()`

This method sets or gets custom data/values associated with the current matched element. It uses the [`dom/data()`](/play-ui/v002/api/dom/data.md) function.

This method sets or gets custom data/values associated with the current matched element. It uses the [`dom/data()`](dom/data.md) function.

```javascript
// Set a value
let instance = $('#el').data('key', value);

// Get a value
let value = $('#el').data('key');
console.log(value);
```

See [`dom/data()`](/play-ui/v002/api/dom/data.md) for details.

### Observers

Observer APIs.

#### `instance.onconnected()`

This method observes when the current matched element is added to the document or given context. It uses the [`dom/connectedCallback()`](/play-ui/v002/api/dom/connectedcallback.md) function.

This method observes when the current matched element is added to the document or given context. It uses the [`dom/connectedCallback()`](dom/connectedcallback.md) function.

```javascript
// Observe "onconnected"
$('#el').onconnected(() => {
    console.log('I am now connected to the DOM!');
});
```

See [`dom/connectedCallback()`](/play-ui/v002/api/dom/connectedcallback.md) for details.

#### `instance.ondisonconnected()`

This method observes when the current matched element is removed from the document or given context. It uses the [`dom/disonconnectedCallback()`](/play-ui/v002/api/dom/disconnectedcallback.md) function.

This method observes when the current matched element is removed from the document or given context. It uses the [`dom/disonconnectedCallback()`](dom/disconnectedcallback.md) function.

```javascript
// Observe "ondisonconnected"
$('#el').ondisonconnected(() => {
    console.log('I am now disconnected from the DOM!');
});
```

See [`dom/disconnectedCallback()`](/play-ui/v002/api/dom/disconnectedcallback.md) for details.

#### `instance.onmutated()`

This method observes when the current matched element is added to, or removed from the document or given context. It uses the [`dom/mutationCallback()`](/play-ui/v002/api/dom/mutationcallback.md) function.

This method observes when the current matched element is added to, or removed from the document or given context. It uses the [`dom/mutationCallback()`](dom/mutationcallback.md) function.

```javascript
// Observe "onmutated"
$('#el').onmutated(connectedState => {
    // If connectedState === 1, connected else if connectedState === 0, disconnected
    console.log('I am now ' + (connectedState ? 'connected to' : 'disconnected from') + ' the DOM!');
});
```

See [`dom/mutationCallback()`](/play-ui/v002/api/dom/mutationcallback.md) for details.

#### `instance.onattrchange()`

This method observes changes on attributes of the current matched element. It uses the [`dom/attrChangeCallback()`](/play-ui/v002/api/dom/attrchangecallback.md) function.

This method observes changes on attributes of the current matched element. It uses the [`dom/attrChangeCallback()`](dom/attrchangecallback.md) function.

```javascript
// Observe "onattrchange"
$('#el').onattrchange(changes => {
    changes.forEach(change => {
        console.log(change);
    });
}, 'attrName');
```

See [`dom/attrChangeCallback()`](/play-ui/v002/api/dom/attrchangecallback.md) for details.

#### `instance.onresize()`

This method observes changes on the size of the current matched element. It uses the [`ui/resizeCallback()`](/play-ui/v002/api/ui/resizecallback.md) function.

This method observes changes on the size of the current matched element. It uses the [`ui/resizeCallback()`](ui/resizecallback.md) function.

```javascript
// Observe "onresize"
$('#el').onresize(newSize => {
    console.log(newSize);
});
```

See [`ui/resizeCallback()`](/play-ui/v002/api/ui/resizecallback.md) for details.
