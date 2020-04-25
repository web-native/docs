# Commons Guide

## Installation

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/commons/dist/main.js"></script>

<script>
// The above tag loads Commons into a global "WebNative" object.
const Commons = window.WebNative.Commons;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/commons
```

#### Import

Commons is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

Commons works both in browser and server environments.

```javascript
// Node-style import
import Commons from '@web-native-js/commons';

// Standard JavaScript import. (Actual path depends on where you installed Commons to.)
import Commons from './node_modules/@web-native-js/commons/src/index.js';
```

### Basic Usage

```javascript
// Arr.flatten
var cities = ['New York City', 'Lagos', 'Berlin', ['two', 'more', 'cities'],];
console.log(Arr.flatten(cities));
```

```javascript
// Obj.each
var cities = {city1: 'New York City', city2: 'Lagos', city3: 'Berlin'};
Obj.each(cities, (key, val) => {
    console.log(key, val);
    // Returning false stops further iteration
    return false;
});
```

