# DOM/selectAll\(\)

This function finds and returns all DOM elements that matche the given CSS selecotr as a `NodeList`. Where no matches are found, an empty `NodeList` is returned.

This works like [`Document.querySelectorAll()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll) / [`Element.querySelectorAll()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelectorAll) but internally polyfills the CSS [`:is()`, \(`:matches()`, `:any()`\) pseudo-class function](https://developer.mozilla.org/en-US/docs/Web/API/CSS/:is).

## Import

```javascript
import selectAll from '@web-native-js/play-ui/src/dom/selectAll.js';
```

## Syntax

```javascript
selectAll(selector[, context = null]);
```

### Parameters

* `selector` - `String`: A valid CSS selector.
* `context` - `HTMLElement`: An optional _node_ under which to run the query. This would be equivalent to `context.querySelectorAll(selector)`.

### Return

* `NodeList` - A _non-live_ `NodeList` containing zero or more matched elements.

## Usage

The example below demonstrates this function with the CSS `:is()` pseudo-class function.

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
let divs = selectAll(':is(header, main) > div');
console.log(divs.length); // 2
```

