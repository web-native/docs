# `add()`
This [Timeline](/firedom/api/ani/Timeline/README.md) instance method adds a new [animation](/firedom/api/ani/Ani/README.md) instance to the timeline.

## Syntax

```js
// Obtain a Timeline instance and call add()
timeline.add(ani);
```

### Parameters
+ `ani` - `Ani`: An instance of [Ani](/firedom/api/ani/Ani/README.md).

### Return
+ `undefined`

## Examples

```js
// The timeline
let timeline  = new Timeline;
// The animation instance
let ani  = new Ani(el, {opacity:0});
// Add
timeline.add(ani);
```