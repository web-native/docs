# add\(\)

This [Timeline](./) instance method adds a new [animation](../ani/) instance to the timeline.

## Syntax

```javascript
// Obtain a Timeline instance and call add()
timeline.add(ani);
```

### Parameters

* `ani` - `Ani`: An instance of [Ani](../ani/).

### Return

* `undefined`

## Examples

```javascript
// The timeline
let timeline  = new Timeline;
// The animation instance
let ani  = new Ani(el, {opacity:0});
// Add
timeline.add(ani);
```

