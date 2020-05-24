# PlayUI API
The PlayUI API can be used individually as standalone static function or together as instance methods.

## PlayUI Core
By design, PlayUI is a collection of standalone functions organized in categories.
* [The DOM](/play-ui/api/dom/README.md) - DOM manipulation APIs.
* [CSS](/play-ui/api/css/README.md) - CSS-processing utilities.
* [Animation](/play-ui/api/ani/README.md) - Animation utilities.
* [Event](/play-ui/api/evt/README.md) - Utilities for binding events and gestures.
* [The UI](/play-ui/api/ui/README.md) - Utilities for manipulating the rendered UI.

## PlayUI Constructible
PlayUI can be used as a *constructible function* (like the jQuery function) with its core functions available as instance methods.

Each of the following instance methods is mapped to a core standalone function and maintains the same function definition as their respective core counterpart. But where a core function defines a DOM element (the subject element) as its first parameter, instance methods implicitly use the element matched by the instance. And where a core function would return the given subject element, an instance method would simply return the instance object.

For the code examples below, create an instance by calling PlayUI with a DOM element or a CSS selector.

```js
// The PlayUI instance
let instance = $('#el');
```

### The DOM
DOM manipulation methods. The most of these methods come in a *sync* and *async* flavour, and a third unsiffixed flavour that is an alias to the *async* method.

#### `instance.htmlAsync()`
This method *asynchronously* sets or gets the HTML content for the current matched element. It uses the [`dom/htmlAsync()`](/play-ui/api/dom/htmlasync.md) function.

```js
// Set the HTML content, do something else afterward
$('#el').htmlAsync('Hello player!').then(instance => {
    // Do something else
});

// Get the HTML content, log it to the console when available
$('#el').htmlAsync().then(content => {
    console.log(content);
});
```

See [`dom/htmlAsync()`](/play-ui/api/dom/htmlasync.md) for details.

#### `instance.htmlSync()`
This method *synchronously* sets or gets the HTML content for the current matched element. It uses the [`dom/htmlSync()`](/play-ui/api/dom/htmlsync.md) function.

```js
// Set the HTML content
let instance = $('#el').htmlSync('Hello player!');

// Get the HTML content, log it to the console
let content = $('#el').htmlSync();
console.log(content);
```

See [`dom/htmlSync()`](/play-ui/api/dom/htmlsync.md) for details.

#### `instance.html()`
This is an alias of the [`instance.htmlAsync()`](#instancehtmlasync) method.

#### `instance.textAsync()`
This method *asynchronously* sets or gets the text content for the current matched element. It uses the [`dom/textAsync()`](/play-ui/api/dom/textasync.md) function.

```js
// Set the text content, do something else afterward
$('#el').textAsync('Hello player!').then(instance => {
    // Do something else
});

// Get the text content, log it to the console when available
$('#el').textAsync().then(content => {
    console.log(content);
});
```

See [`dom/textAsync()`](/play-ui/api/dom/textasync.md) for details.

#### `instance.textSync()`
This method *synchronously* sets or gets the text content for the current matched element. It uses the [`dom/textSync()`](/play-ui/api/dom/textsync.md) function.

```js
// Set the text content
let instance = $('#el').textSync('Hello player!');

// Get the text content, log it to the console
let content = $('#el').textSync();
console.log(content);
```

See [`dom/textSync()`](/play-ui/api/dom/textsync.md) for details.

#### `instance.text()`
This is an alias of the [`instance.textAsync()`](#instancetextasync) method.

#### `instance.appendAsync()`
This method *asynchronously* appends text content to the current matched element. It uses the [`dom/appendAsync()`](/play-ui/api/dom/appendasync.md) function.

```js
// Append content, do something else afterward
$('#el').appendAsync('Hello player!').then(instance => {
    // Do something else
});
```

See [`dom/appendAsync()`](/play-ui/api/dom/appendasync.md) for details.

#### `instance.appendSync()`
This method *synchronously* appends text content to the current matched element. It uses the [`dom/appendSync()`](/play-ui/api/dom/appendsync.md) function.

```js
// Append content
let instance = $('#el').appendSync('Hello player!');
```

See [`dom/appendSync()`](/play-ui/api/dom/appendsync.md) for details.

#### `instance.append()`
This is an alias of the [`instance.appendAsync()`](#instanceappendasync) method.

#### `instance.prependAsync()`
This method *asynchronously* prepends text content to the current matched element. It uses the [`dom/prependAsync()`](/play-ui/api/dom/prependasync.md) function.

```js
// Prepend content, do something else afterward
$('#el').prependAsync('Hello player!').then(instance => {
    // Do something else
});
```

See [`dom/prependAsync()`](/play-ui/api/dom/prependasync.md) for details.

#### `instance.prependSync()`
This method *synchronously* prepends text content to the current matched element. It uses the [`dom/prependSync()`](/play-ui/api/dom/prependsync.md) function.

```js
// Prepend content
let instance = $('#el').prependSync('Hello player!');
```

See [`dom/prependSync()`](/play-ui/api/dom/prependsync.md) for details.

#### `instance.prepend()`
This is an alias of the [`instance.prependAsync()`](#instanceprependasync) method.

#### `instance.attrAsync()`
This method *asynchronously* sets or gets an attribute for the current matched element. It uses the [`dom/attrAsync()`](/play-ui/api/dom/attrasync.md) function.

```js
// Set an attribute, do something else afterward
$('#el').attrAsync('name', 'value').then(instance => {
    // Do something else
});

// Get an attribute, log it to the console when available
$('#el').attrAsync('name').then(value => {
    console.log(value);
});
```

See [`dom/attrAsync()`](/play-ui/api/dom/attrasync.md) for details.

#### `instance.attrSync()`
This method *synchronously* sets or gets an attribute for the current matched element. It uses the [`dom/attrSync()`](/play-ui/api/dom/attrsync.md) function.

```js
// Set an attribute
let instance = $('#el').attrSync('name', 'value');

// Get an attribute, log it to the console
let value = $('#el').attrSync('name');
console.log(value);
```

See [`dom/attrSync()`](/play-ui/api/dom/attrsync.md) for details.

#### `instance.attr()`
This is an alias of the [`instance.attrAsync()`](#instanceattrasync) method.

#### `instance.classAsync()`
This method is used to *asynchronously* set/unset the *class* attribute or add/remove entries on the *class* list for the current matched element. It uses the [`dom/classAsync()`](/play-ui/api/dom/classasync.md) function.

```js
// Add a class, do something else afterward
$('#el').classAsync('class1', true).then(instance => {
    // Do something else
});

// Remove a class
$('#el').classAsync('class1', false).then(instance => {
    // Do something else
});
```

See [`dom/classAsync()`](/play-ui/api/dom/classasync.md) for details.

#### `instance.classSync()`
This method is used to *synchronously* set/unset the *class* attribute or add/remove entries on the *class* list for the current matched element. It uses the [`dom/classSync()`](/play-ui/api/dom/classsync.md) function.

```js
// Add a class
let instance = $('#el').classAsync('class1', true);

// Remove a class
let instance = $('#el').classAsync('class1', false);
```

See [`dom/classSync()`](/play-ui/api/dom/classsync.md) for details.

#### `instance.class()`
This is an alias of the [`instance.classAsync()`](#instanceclassasync) method.

### CSS
CSS-processing utilities. A few of these methods come in a *sync* and *async* flavour, and a third unsiffixed flavour that is an alias to the *async* method.

#### `instance.cssAsync()`
This method *asynchronously* sets or gets CSS rules for the current matched element. It uses the [`css/cssAsync()`](/play-ui/api/css/cssasync.md) function.

```js
// Set a CSS rule, do something else afterward
$('#el').cssAsync('color', 'blue').then(instance => {
    // Do something else
});

// Get a CSS rule, log it to the console when available
$('#el').cssAsync('color').then(value => {
    console.log(value);
});
```

See [`css/cssAsync()`](/play-ui/api/css/cssasync.md) for details.

#### `instance.cssSync()`
This method *synchronously* sets or gets CSS rules for the current matched element. It uses the [`css/cssSync()`](/play-ui/api/css/csssync.md) function.

```js
// Set a CSS rule
let instance = $('#el').cssSync('color', 'blue');

// Get a CSS rule, log it to the console
let value = $('#el').cssSync('color');
console.log(value);
```

See [`css/cssSync()`](/play-ui/api/css/csssync.md) for details.

#### `instance.css()`
This is an alias of the [`instance.cssAsync()`](#instancecssasync) method.

#### `instance.cssInline()`
This method returns one or more style properties from the current matched element's style attribute. It uses the [`css/readInline()`](/play-ui/api/css/readinline.md) function.

```js
// Read an inline CSS rule
let value = $('#el').cssInline('color');
console.log(value);
```

See [`css/readInline()`](/play-ui/api/css/readinline.md) for details.

#### `instance.cssStylesheet()`
This method returns one or more style properties associated with the current matched element accross the document's stylesheets. It uses the [`css/readStylesheet()`](/play-ui/api/css/readstylesheet.md) function.

```js
// Read a CSS rule from stylesheet
let value = $('#el').cssStylesheet('color');
console.log(value);
```

See [`css/readStylesheet()`](/play-ui/api/css/readstylesheet.md) for details.

#### `instance.cssTransaction()`
This method establishes a CSS operation on the current matched element that can be rolledback safely. It uses the [`css/transaction()`](/play-ui/api/css/transaction.md) function.

```js
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

See [`css/transaction()`](/play-ui/api/css/transaction.md) for details.

### Animation
Animation utilities.

#### `instance.play()`
This method creates and play an animation on the current matched element. It uses the [`ani/play()`](/play-ui/api/ani/play.md) function.

```js
// Play fadeout
$('#el').play([{opacity:1}, {opacity:0}], {duration:600}).then(instance => {
    console.log('The end!');
});
```

See [`ani/play()`](/play-ui/api/ani/play.md) for details.

### Event
Utilities for binding events and gestures.

#### `instance.on()`
This method binds an event/gesture handler to the current matched element. It uses the [`evt/on()`](/play-ui/api/evt/on.md) function.

```js
// Bind a "doubletap" gesture handler
$('#el').on('doubletap', event => {
    alert('You doubletapped me!');
});
```

See [`evt/on()`](/play-ui/api/evt/on.md) for details.

#### `instance.off()`
This method unbinds event/gesture handlers oreviously bound using the [`instance.on()`](#instanceon) method. It uses the [`evt/off()`](/play-ui/api/evt/off.md) function.

```js
// Unbind "doubletap" gesture handlers
$('#el').off('doubletap';
```

See [`evt/off()`](/play-ui/api/evt/off.md) for details.

#### `instance.trigger()`
This method triggers event/gesture handlers oreviously bound using the [`instance.on()`](#instanceon) method. It uses the [`evt/trigger()`](/play-ui/api/evt/trigger.md) function.

```js
// Unbind "doubletap" gesture handlers
$('#el').trigger('doubletap';
```

See [`evt/trigger()`](/play-ui/api/evt/trigger.md) for details.

### Other
Utilities for cross-cutting concerns.

#### `instance.data()`
This method sets or gets custom data/values associated with the current matched element. It uses the [`dom/data()`](/play-ui/api/dom/data.md) function.

```js
// Set a value
let instance = $('#el').data('key', value);

// Get a value
let value = $('#el').data('key');
console.log(value);
```

See [`dom/data()`](/play-ui/api/dom/data.md) for details.

### Observers
Observer APIs.

#### `instance.onconnected()`
This method observes when the current matched element is added to the document or given context. It uses the [`dom/connectedCallback()`](/play-ui/api/dom/connectedcallback.md) function.

```js
// Observe "onconnected"
$('#el').onconnected(() => {
    console.log('I am now connected to the DOM!');
});
```

See [`dom/connectedCallback()`](/play-ui/api/dom/connectedcallback.md) for details.

#### `instance.ondisonconnected()`
This method observes when the current matched element is removed from the document or given context. It uses the [`dom/disonconnectedCallback()`](/play-ui/api/dom/disconnectedcallback.md) function.

```js
// Observe "ondisonconnected"
$('#el').ondisonconnected(() => {
    console.log('I am now disconnected from the DOM!');
});
```

See [`dom/disconnectedCallback()`](/play-ui/api/dom/disconnectedcallback.md) for details.

#### `instance.onmutated()`
This method observes when the current matched element is added to, or removed from the document or given context. It uses the [`dom/mutationCallback()`](/play-ui/api/dom/mutationcallback.md) function.

```js
// Observe "onmutated"
$('#el').onmutated(connectedState => {
    // If connectedState === 1, connected else if connectedState === 0, disconnected
    console.log('I am now ' + (connectedState ? 'connected to' : 'disconnected from') + ' the DOM!');
});
```

See [`dom/mutationCallback()`](/play-ui/api/dom/mutationcallback.md) for details.

#### `instance.onattrchange()`
This method observes changes on attributes of the current matched element. It uses the [`dom/attrChangeCallback()`](/play-ui/api/dom/attrchangecallback.md) function.

```js
// Observe "onattrchange"
$('#el').onattrchange(changes => {
    changes.forEach(change => {
        console.log(change);
    });
}, 'attrName');
```

See [`dom/attrChangeCallback()`](/play-ui/api/dom/attrchangecallback.md) for details.

#### `instance.onresize()`
This method observes changes on the size of the current matched element. It uses the [`ui/resizeCallback()`](/play-ui/api/ui/resizecallback.md) function.

```js
// Observe "onresize"
$('#el').onresize(newSize => {
    console.log(newSize);
});
```

See [`ui/resizeCallback()`](/play-ui/api/ui/resizecallback.md) for details.
