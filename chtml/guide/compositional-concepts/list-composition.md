# List Composition

It is intuitive to make a list in CHTML! Lists can be composed either programmatically or declaratively.

## Using the Chtml.populate\(\) Instance Method

This method is used to populate a list component programmatically. It takes a data component to populate and a namespace for importing children into the list component. Now for each item in the data component, a child element is imported and appended to the list component.

Here are the list and list-item elements.

```markup
<html>
  <head>
  <template is="chtml-bundle">

      <ul data-namespace="html/list "></ul>
      <li data-namespace="html/listitem"></li>

  </template>
  </head>

  <body>

    <!--import the list here -->
    <chtml-import id="list-component" data-namespace="html/list"></chtml-import>

  </body>
</html>
```

The import element above should be automatically resolved. So the body should have a `<ul>` element by now. Now, we'll populate this element with data. An import will be made for each data item.

```javascript
let listComponent = new Chtml(document.querySelector('#list-component'));
listComponent.populate(['item-1', 'item-2'], 'html/listitem');
```

This should produce the following:

```markup
<ul id="list-component" data-namespace="html/list">
  <li data-namespace="html/listitem"></li>
  <li data-namespace="html/listitem"></li>
</ul>
```

An object would as well produce the same result:

```javascript
listComponent.populate({key1:'item-1', key2:'item-2'}, 'html/listitem');
```

To manually set data on list items as they are loaded, provide a third argument as a callback function. This function will receive the just-imported list item and the data item.

```javascript
listComponent.populate(['item-1', 'item-2'], 'html/listitem', (listItem, dataItem, itemKey) => {
    listItem.innerHtml = dataItem;
});
```

Now we should have something like the following:

```markup
<ul id="list-component" data-namespace="html/list">
  <li data-namespace="html/listitem">item-1</li>
  <li data-namespace="html/listitem">item-2</li>
</ul>
```

To achieve this declaratively, we could simply declare the bindings on the list-item elements themselves or on the import element that imports them.

```markup
<template is="chtml-bundle">

  <li data-namespace="html/listitem">

    <script type="text/scoped-js">
      html(model);
    </script>

  </li>

</template>
```

Now, as a list item lands in the list component, its directives are automatically executed. It would like the implementation below:

```javascript
listComponent.populate(['item-1', 'item-2'], 'html/listitem', (listItem, dataItem) => {
  let itemComponent = new Chtml(listItem);
    itemComponent.bind(dataItem);
});
```

## Using an Extended Namespace

The list above can be all declaratively-rendered! CHTML supports using a two-part namespace to import a list component. The first part of the namespace serves as the actual import namespace, while the second is stripped off for subsequently importing children. Two forward slashes separate these namespaces.

```markup
<body>

  <!--import the list here -->
  <chtml-import id="list-component" data-namespace="html/list//html/listitem"></chtml-import>

</body>
```

Now we can simply set the data to the list component and have things automatically populated; every component loaded with a two-part namespace is automatically handled this way.

```javascript
listComponent.bind(['item-1', 'item-2']);
// Or put transparently
//list.model = ['item-1', 'item-2'];
```

## Updating a List

List population is implement in CHTML as a binding contract between the DOM and data components. So more list items are imported as needed to match the number of data items received.

Below, we render two extra items to our list. Since two list-item elements already exists in the list component, they will simply be re-rendered. Only two new imports will now be made.

```javascript
list.model = ['item-1', 'item-2', 'item-3', 'item-4'];
```

But what happens if we reduced the list data to a single item after having rendered many? The number of list-item elements will also be reduced! And setting an empty array will as well empty the list component. Remember, this is a binding contract!

```javascript
list.model = ['item-1',];
```

Now, our component should be:

```markup
<ul id="list-component" data-namespace="html/list">
  <li data-namespace="html/listitem">item-1</li>
</ul>
```

Really, instead of replacing the entire list and making the component render from the beginning, we can selectively add/remove items. This works naturally with Reflex as CHTML is based on reflex actions.

```javascript
var listData = ['item-1', 'item-2',];
list.model = listData;

Reflex.set(listData, 2, 'item-3');
Reflex.set(listData, 3, 'item-4');
```

We can directly call the observed array's prototype methods with the help of Reflex. And the component will be rendering accordingly.

```javascript
// Charge the array's push(), splice() and unshift() methods with Reflex actions
Reflex.init(listData, ['push', 'splice', 'unshift']);
// Add a fourth item
listData.push('item-4');
// Empty the array
listData.splice(0);
// Add a new first item
listData.push('new item-1');
```

List rendering is index-aware! Items can be added to the data component on any key at any time, yet imported list-item elements will be correctly placed on the right index.

```javascript
Reflex.set(listData, 10, 'item-11');
Reflex.set(listData, 8, 'item-9');
listData.unshift('newest item-1');
```

The last item `newest item-1` should still come first, and `item-8` should still be placed before `item-10`:

```markup
<ul id="list-component" data-namespace="html/list">
  <li data-namespace="html/listitem">newest item-1</li>
  <li data-namespace="html/listitem">new item-1</li>
  <li data-namespace="html/listitem">item-8</li>
  <li data-namespace="html/listitem">item-10</li>
</ul>
```

## Implementing Sub-Namespaces

When populating a list, items are retrieved, as seen, using the sub-namespace given. But individual items are retrieved with a namespace that is uniquely derived for the item. CHTML appends the item`s key to the base sub-namespace to import an item. So in the population we made above, the first item was imported using the namespace`html/listitem/0`, and the second item was imported using the namespace`html/listitem/1\`, and so on.

The same pattern holds true for object-type list data. So for an object like `{key1:`item-1`, key2:`item-2`}`, the first item will be imported as `html/listitem/key1`, and the second, `html/listitem/key2`.

Item-specific components can thus be implemented under these derived namespaces where items really need to be unique. The base namespace, `html/listitem` in our case, remains the source of every item component imported where an item-specific component isn't implemented.

Item namespaces can also be more dynamically derived using placeholders along the namespace path. Placeholders are enclosed in square brackets `[]` as variables that reference properties of the given item. Here's how this could look:

```markup
<body>

  <!--import the list here -->
  <chtml-import id="list-component" data-namespace="html/list//html/listitem/[name]"></chtml-import>

</body>
```

The `name` property in each item below will influence how an item's namespace is derived. And we would have also been able to reference deep item properties using the dot \(.\) notation.

```javascript
listComponent.bind([{name:'item-1', value:'Hello World!'}, {name:'item-2', value:'Bonjour!'}]);
```

