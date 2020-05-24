# Scoped CSS
ScopedCSS is is currently under development. Please submit an issue on our github repo should you have a suggestion.

Attempting to access scoped elements using the regular methodologies can be very challenging. *Scope Accessors* are a set of CSS selecotrs and JavaScript APIs that provide a straight-forward way get to these elements.

## On this page:
+ [Scope Selectors](#scope-selectors)
  + [Scoped CSS](#scoped-css)
  + [The Scope Selector API](#the-scope-selector-api)

## Scope Selectors
By default, regular CSS selectors work as exepcted when run on a scope from a stylesheet or the regular `querySelector()` and `querySelectorAll()` DOM methods. A few other selectors, however, have been created or redesigned specifically for accessing scoped elements. These selectors are only available in *scoped CSS* and the new [Scope Selector API](#the-scope-selector-api).
+ **The `:scope` Pseudo Class** - This is a regular selector that always matches the element on which it is run.
+ **The `:root` Pseudo Class** - When run within a scope, this regular selector works to match the scope's root element.
+ **The Scoped-ID `#` Selector** - When run on a scope root, this regular selector works differently to match elements by `scoped:id` instead of `id`. (`scoped:id` is a scope's own ID attribute.)
+ **The Scoped-Tag `@` Selector** - This selector works to match elements that are semantically scoped to the element on which it is called.
+ **The Path `/` Notation** - This selector is used to access nested scopes.

The behaviour of these selectors becomes clearer as we put them to use in Scoped CSS and the Scope Selector API.

### Scoped CSS

Scoped CSS is made available by the [*ScopedCSS* JavaScript library](/chtml/v060/guide/installation.md).

### The Scope Selector API
Scoped HTML introduces two new methods, the `scopeSelector()` and  `scopeSelectorAll()` methods, for querying a scope using CSS selectors, much like the `querySelector()` and  `querySelectorAll()` methods. The distinguishing factor is that only elements within the boundaries of the scope are matched. Additionally, the ID selector `#` is mapped to the `scoped:id` attribute instead of the `id` attribute.

This API is made available by the [*ScopedHTML* JavaScript library](/chtml/v060/guide/installation.md).

```js
// Let's retrieve the #article element itself using document.querySelector()
let article = document.querySelector('#article');

// Let's query elements by "scoped-id"
let articleTitle = article.scopeSelector('#title');
let articleContent = article.scopeSelector('#content');

// Chained queries would just take us into nested scopes
let articleAuthorName = article.scopeSelector('#author').scopeSelector('#name');
```
