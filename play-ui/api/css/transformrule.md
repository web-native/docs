# CSS/TransformRule

This class is used to parsing and stringifying the CSS transform rule.

## Import

```javascript
import TransformRule from '@web-native-js/play-ui/src/css/TransformRule.js';
```

## `TransformRule.parse()`

This function parses and returns the element's computed transform rule as an object tree.

### Syntax

```javascript
let transformRuleObject = TransformRule.parse(rule);
```

#### Parameters

* `rule` - `String`: A CSS transform rule.

#### Return

* `TransformRule` - The parsed transform rule.

## Examples

```markup
<style>
div {
    transform: translate(30, 40) scale(3);
}
</style>
<div id="el"></div>
```

```javascript
let transformRule = document.querySelector('#el').style.transform;

// Set attribute
let transformRuleObject = TransformRule.parse(transformRule);

// Show
console.log(transformRuleObject);
/**
{
    translate: [30, 40],
    sclae: 3,
}
*/

//Convert to string
console.log(transformRuleObject.toString());
// translate(30, 40) scale(3)
```

We can also create a CSS transfrom rule from an object.

```javascript
let transformRuleObject = new TransformRule({
    translate: [30, 40],
    sclae: 3,
});

//Convert to string
console.log(transformRuleObject.toString());
// translate(30, 40) scale(3)
```

