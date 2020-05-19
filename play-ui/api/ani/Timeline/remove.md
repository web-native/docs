# `remove()`
This [Timeline](/play-ui/api/ani/Timeline/README.md) instance method removes an [animation](/play-ui/api/ani/Ani/README.md) instance from the timeline.

## Syntax

```js
// Obtain a Timeline instance and call remove()
timeline.remove(ani);
```

### Parameters
+ `ani` - `Ani`: An instance of [Ani](/play-ui/api/ani/Ani/README.md).

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