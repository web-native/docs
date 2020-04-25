# `remove()`
This [Timeline](/firedom/api/ani/Timeline/README.md) instance method removes an [animation](/firedom/api/ani/Ani/README.md) instance from the timeline.

## Syntax

```js
// Obtain a Timeline instance and call remove()
timeline.remove(ani);
```

### Parameters
+ `ani` - `Ani`: An instance of [Ani](/firedom/api/ani/Ani/README.md).

### Return
+ `undefined`

## Examples

```js
// The animation instance
let ani  = new Ani(el, {opacity:0});
// The timeline
let timeline  = new Timeline([ani]);

// Add
timeline.remove(ani);
```