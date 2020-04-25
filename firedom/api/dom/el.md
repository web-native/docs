# `DOM/el()`

This function translates a variety of input types to an element. You can supply a CSS selector or an HTML markup and get an `HTMLElement` in return.

## Import

```javascript
import el from '@web-native-js/firedom/src/dom/el.js';
```

## Syntax

```javascript
el(input);
```

### Parameters

* `input` - `String|HTMLElement`: A valid CSS selector, HTML markup or an instance of `HTMLElement`. If a selector is provided, only the first-matched element is returned.

### Return

* `HTMLElement` - The resulting DOM element.
* `NULL` - Where _input_ is a CSS selector but matches no element.

## Examples

In the example below, we are calling this function with a CSS selector.

```markup
<body>

  <header>
    <div>DIV1</div>
  </header>

  <main>
    <div>DIV2</div>
  </main>

</body>
```

```javascript
let div = el('div');
console.log(div.innerText); // DIV1
```

