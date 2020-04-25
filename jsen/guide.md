# JSEN Guide

## Installation

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/jsen/dist/main.js"></script>

<script>
// The above tag loads JSEN into a global "WebNative" object.
const Jsen = window.WebNative.Jsen;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/jsen
```

#### Import

JSEN is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

JSEN works both in browser and server environments.

```javascript
// Node-style import
import Jsen from '@web-native-js/jsen';

// Standard JavaScript import. (Actual path depends on where you installed JSEN to.)
import Jsen from './node_modules/@web-native-js/jsen/src/index.js';
```

## Examples

### Math expression

```javascript
// parse() a Math expression
var expr = '7 + 8';
var exprObj = Jsen.parse(expr);

// get result with eval()
console.log(exprObj.eval());

// convert to string anytime
console.log(exprObj.toString());
```

### Primitives and object notations

```javascript
// parse() a value
var expr = '10';
var exprObj = Jsen.parse(expr);

// get result with eval()
console.log(exprObj.eval());

// ------------

// parse() an array expression
var expr = '["New York City", "Lagos", "Berlin"]';
var exprObj = Jsen.parse(expr);

// get result with eval()
console.log(exprObj.eval());

// ------------

// parse() an object expression
var expr = '{city1: "New York City", city2: "Lagos", city3: "Berlin"}';
var exprObj = Jsen.parse(expr);

// get result with eval()
console.log(exprObj.eval());

// ------------

// convert to string anytime
console.log(exprObj.toString());
```

### Conditional Expressions

```javascript
// "if" ... "else" constructs
var expr = 'if (2 > 1) {
    // Expression1
} else if (3 > 2) {
    // Expression2
} else {
    // Expression3
}';

// Ternary Conditional Expressions;
var expr = '2 > 1 ? expression1 : (3 > 2 ? expression2 : expression3)';
```

### Eval\(\) with a context object

```javascript
// parse() a logical expression with references to properties of a context object
var expr = 'age < 18 ? fname + " " + lname + " does not meet the age requirement!" : fname + " " + lname + " is old enough!"';
var exprObj = Jsen.parse(expr);

// eval() with a context object
var context = {fname: "John", lname: "Doe", age: 24};
console.log(exprObj.eval(context));

// eval() same exprObj with another context object
var context2 = {fname: "John2", lname: "Doe2", age: 10};
console.log(exprObj.eval(context2));

// convert to string anytime
console.log(exprObj.toString());
```

#### Eval\(\) with traps

The `eval()` allows us to set traps that intercept `set`, `get`, `delete`, and `in` operations. A trap works just like ES6 Proxy trap.

```javascript
// The "get" handler in the trap below will be called as the "age", "fname", "lname" references are being evaluated
console.log(exprObj.eval(context2, {
    get:(target, propertyName) => {
        return target[propertyName];
    }
}));
```

### Call a method

```javascript
// parse() a logical expression with embedded calls
var expr = '"Today is: " + date().toString()';
var exprObj = Jsen.parse(expr);

// eval() with the given context
var context = {date:() => (new Date)};
console.log(exprObj.eval(context));

// convert to string anytime
console.log(exprObj.toString());
```

### ES6-style function declaration supported

```javascript
// parse() a function declaration that will materialize into a real function
var expr = '(arg1, arg2) => {return arg1 + arg2 + argFromContext}';
var exprObj = Jsen.parse(expr);

// eval() to materialize the declared function
var context = {argFromContext: 10};
var sumFunction = exprObj.eval(context);

// Call materialized function now
console.log(sumFunction(30, 20));

// convert to string anytime
console.log(exprObj.toString());
```

### Assignment operation supported with the "mutates" flag set to true

```javascript
// parse() an assignment expression.
// Multiple expressions supported.
var expr = 'prop2 = "val2"; prop3 = "val3"';
var exprObj = Jsen.parse(expr, {mutates: true});

// eval() with a context object
var context = {prop1: "val1"};
console.log(exprObj.eval(context));

// Confirm the new properties have been set
console.log(context);

// convert to string anytime
console.log(exprObj.toString());
```

### Delete operation supported with the "mutates" flag set to true

```javascript
// parse() a delete expression.
var expr = 'delete prop1; delete prop1';
var exprObj = Jsen.parse(expr, {mutates: true});

// eval() with a context object to mutate
var context = {prop1: "val1", prop2: "val2", prop3: "val3"};
console.log(exprObj.eval(context));

// Confirm the new property has been deleted
console.log(context);

// convert to string anytime
console.log(exprObj.toString());
```

### Multiple expressions and Comments supported

```javascript
// Line comments
var expr = `

/**
 * Block comments
 */

// Single line comments
delete prop1; delete /*comment anywhere*/prop3;
`;

var exprObj = Jsen.parse(expr, {mutates: true});
// To obtain the comments...
let {comments} = Jsen.lex(expr, {mutates: true});
console.log(comments);

// eval() with a context object to mutate
var context = {prop1: "val1", prop2: "val2", prop3: "val3"};
console.log(exprObj.eval(context));

// Confirm the new properties have been deleted
console.log(context);

// convert to string anytime
console.log(exprObj.toString());
```

