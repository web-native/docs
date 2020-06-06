# remove\(\)

This [Timeline](./) instance method removes an [animation](../ani/) instance from the timeline.

## Syntax

```javascript
// Obtain a Timeline instance and call remove()
timeline.remove(ani);
```

### Parameters

* `ani` - `Ani`: An instance of [Ani](../ani/).

### Return

* `undefined`

## Examples

```javascript
// The animation instance
let ani  = new Ani(el, {opacity:0});
// The timeline
let timeline  = new Timeline([ani]);

// Add
timeline.remove(ani);
```

