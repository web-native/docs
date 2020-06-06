# DOM/select\(\)

This function finds and returns the first DOM element that matches the given CSS selecotr. Where no matches are found, `NULL` is returned.

This works like [`Document.querySelector()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) / [`Element.querySelector()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector) but internally polyfills the CSS [`:is()`, \(`:matches()`, `:any()`\) pseudo-class function](https://developer.mozilla.org/en-US/docs/Web/API/CSS/:is).

## Import

```javascript
import select from '@web-native-js/play-ui/src/dom/select.js';
```

## Syntax

```javascript
select(selector[, context = null]);
```

### Parameters

* `selector` - `String`: A valid CSS selector.
* `context` - `HTMLElement`: An optional _node_ under which to run the query. This would be equivalent to `context.querySelector(selector)`.

### Return

* `HTMLElement` - The matched element.
* `NULL` - if no matches were found.

## Examples

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
let div = select(':is(header, main) > div');
console.log(div.innerText); // DIV1
```

