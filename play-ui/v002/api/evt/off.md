# `off()`
This function unbinds event/gesture handlers previously bound using the [`on()`](/play-ui/v002/api/evt/on.md) function.

## Import

```js
import off from '@web-native-js/play-ui/src/evt/off.js';
```

## Syntax

```js
// Unbind all listeners bound to the following event name
// regardless of the event handler
off(el, eventName);

// Unbind the listener bound with the following event handler  
off(el, eventName, originalHandler);

// Unbind the listener bound with the following event handler and tag 
off(el, eventName, originalHandler, {tags:[...originalTags]});

// Unbind the listener bound with the following tag 
// regardless of the event handler
off(el, eventName, null, {tags:[...originalTags]});
```

**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `eventName` - `String`: The event name.
+ `originalHandler` - `Function`: The handler function originally used with `on()`.
+ `params` - `Object`: Additional parameters.

**Return**
+ `undefined`

## Usage

```js
// Unbind a specific listenr bound to doubletap
off(document.body, 'doubletap', originalHandler);

// Unbind all listenrs bound to doubletap
off(document.body, 'doubletap');
```