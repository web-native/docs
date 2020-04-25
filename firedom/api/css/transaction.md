# `transaction()`
This function establishes a CSS operatiom that can be rolledback safely. It is a convenient way to use the Firedom's [Transaction](/firedom/api/transaction.md) class.

## Import

```js
import transaction from '@web-native-js/firedom/src/css/transaction.js';
```

## Syntax

```js
// Single property transaction
let transactionInstance = transaction(el, name);

// Multiple properties transaction
let transactionInstance = transaction(el, [name]);
```

#### Parameters
+ `el` - `HTMLElement`: The source DOM element.
+ `name` - `String|Array`: The CSS property or list of properties that should be rolled back after the transaction.

#### Return
+ `Transaction` - The [Transaction](/firedom/api/transaction.md) instance.

## Examples

```html
<style>
div {
    background-color: yellow;
}
</style>
<div id="el" style="color:red"></div>
```

```js
let el = document.querySelector('#el');

// Obtaine a Transaction instance
let transactionInstance = transaction(el, ['background-color', 'color']);
// Create a savepoint - savepoint1
// We can rollback to this point later
transactionInstance.save();

// Restyle the element
el.style.color = green;
el.style.backgroundColor = black;
// Create a savepoint - savepoint2
// We can rollback to this point later
transactionInstance.save();

// Restyle the element again
el.style.color = brown;
el.style.backgroundColor = teal;
// Create a savepoint - savepoint3
// We can rollback to this point later
transactionInstance.save();

// Rollback to anypoint after some time
setTimeout(() => {
    // Rollback all the way to the element's initial state
    transactionInstance.rollback(0);
    // Now background-color should fallback to yellow
    // And color should be back to red
}, 2000);
```

In the code above, if another part of the app had changed one of those properties, then *rollback* would have left that property untouched to respect the other code.

```js
// Obtaine a Transaction instance
let transactionInstance = transaction(el, ['background-color', 'color']);
// Create a savepoint - savepoint1
// We can rollback to this point later
transactionInstance.save();

// Restyle the element again
el.style.color = green;
el.style.backgroundColor = black;
// Create a savepoint - savepoint2
// We can rollback to this point later
transactionInstance.save();

// This is another code setting their own color
// after we have saved a rollback point
el.style.color = brown;


// Rollback to savepoint2
setTimeout(() => {
    // Rollback all the way to the element's initial state
    transactionInstance.rollback(0);
    // Now background-color should be rolled back to black
    // But color will remain brown
}, 2000);
```
