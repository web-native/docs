# Imports and Recomposition, Inheritance and Cascading

CHTML makes it seamless to reuse a defined component either in whole or in part. This helps us build entire applications with much fewer components.

## Imports

CHTML imports are a way to place any component anywhere in a HTML document. Here, a temporary element is used to import the needed element or component. This temporary element is the `chtml-import` element.

```markup
<chtml-import data-namespace="html/content/article/readonly"></chtml-import>
```

Imports use the `data-namespace` attribute to reference their source element. The import element itself gets replaced by the imported element; so at best, an import element is only a placeholder.

Using imports, we can easily compose a larger component with smaller components. An `article` component, for example, can now simply import a user component as its `author` node.

```markup
<template is="chtml-bundle">

  <div data-role="user" data-namespace="html/badge/user">
    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>
  </div>

  <div data-role="article" data-namespace="html/content/article/readonly">
    <div>
      <chtml-import data-role="article-author" data-namespace="html/badge/user"></chtml-import>
        <div>
          <div data-role="article-content"></div>
        </div>
    </div>
  </div>

</template>
```

Actually, anything can be imported - anything tag-based - as long it is placed in a CHTML bundle and has a namespace. So although we like to refer to things as components, a more general term for them would be _modules_ - CHTML modules. A bundle is composed of modules.

On this general note, a bundle could look like this:

```markup
<template is="chtml-bundle">

  <!-- components -->
  <div data-role="user" data-namespace="html/badge/user">
    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>
  </div>

  <!-- Media: images, videos, etc -->
  <img src="/assets/img/brand/logo.png" data-namespace="img/brand/logo" />

  <!--  Media with data-URLs. -->
  <!-- Good for preloading resources -->
  <img src="data:image/png,%89PNG%0D%0A=" data-namespace="img/brand/logo2" />

  <!-- SVG, possibly SVG icons -->
  <svg data-namespace="svg/brand/logo/24x24" >
    <path />
  </svg>

  <!-- Stylesheets -->
  <style type="text/css" data-namespace="css/badge" >
  /* rules */
  </style>

</template>
```

Now we can import them as needed:

```markup
<html>

  <head>
  <!-- 
  If we had created the bundle as a html file, we link to it here
  <template is="chtml-bundle" src="/path/to/bundle.html"></template>
  -->
  </head>

  <body>
    <!-- Place an image here -->
  <chtml-import data-namespace="img/brand/logo2"></chtml-import>
  </body>

</html>
```

### Imports Ondemand

By default, imports are resolved as soon as the DOM comes to life. This makes sense most of the times. At other times, we may want imports to resolve _on-demand_ - just at the time they are accessed in an application. This is achieved with the `ondemand` _Boolean_ attribute.

When implemented as a node in a component, for example, imports of this type are provisional and get resolved on first access to the node.

```markup
<div data-role="article">
  <chtml-import ondemand data-role="article-author" data-namespace=" html/badge/user"></chtml-import>
</div>
```

### Shadow Imports

It is possible to import components directly into an element's shadow DOM. This form of import is called shadow import. Shadow imports are qualified with the `shadow` _Boolean_ attribute. An import's shadow host is its immediate parent.

The import element itself gets replaced and is never part of the shadow DOM nor the light DOM. Imported components are never visible anywhere in the DOM but live hidden in the host element's shadow DOM.

```markup
<div id="host">
  <chtml-import shadow data-namespace=" html/badge/user"></chtml-import>
</div>
```

We could even send some CSS into the shadow DOM for the component.

```markup
<div id="host">
  <chtml-import shadow data-namespace=" html/badge/user"></chtml-import>
  <chtml-import shadow data-namespace="css/badge"></chtml-import>
</div>
```

## Recomposition

Import elements can do more than just place a component. They can be empowered to recompose the component being imported. We do this by predefining attributes and content on the import element with a view to having these on the component they import. The import element is replaced as usual, but this time, with a richly composed component.

For example, if we wanted an incoming component to have an ID, we would set this on the import element.

```markup
<chtml-import id="some-id" data-namespace="html/badge/user"></chtml-import>
```

We could import the same component in another place with a different ID.

```markup
<chtml-import id="some-other-id" data-namespace="html/badge/user"></chtml-import>
```

Recomposition helps us fine-tune imported components to fit perfectly into every use-case. The component below, for example, could be recomposed on every import we make of it.

```markup
<template is="chtml-bundle">

  <div data-role="user" data-namespace="html/badge/user" style="color:black">
    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>
  </div>

</template>
```

### Attribute-Based Recomposition

Certain attributes can be predefined on an import element with a view to having them on the imported component. For some types of attribute, values are replaced on the component; for others, values are merged.

**Classes and Inline CSS:** Classes that may be found on an import element will be merged into the component's `class` attribute. Also, any inline CSS rules found will be appended to the component's `style` attribute; CSS cascading takes effect naturally, with rules from the import element taking priority.

```markup
<div data-role="article">
  <chtml-import class="class1 class2" style="color:blue" data-namespace="components/badge/user"></chtml-import>
</div>
```

The final composition gives us:

```markup
<div data-role="article">
  <div data-role="user" class="class1 class2" style="color:black; color:blue" data-namespace="components/badge/user">
    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>
  </div>
</div>
```

**Roles and Component Hints:** Any ARIA roles \(in the `role` attribute\) and CHTML roles \(in the `data-role` attribute\) found on an import element will be merged into their respective attributes on the component; so an imported component can take on new roles to play. In the same way, any _hints_ found on the import element will be merged into the component's `data-tree` attribute.

Below, we're importing the _user_ component to be an _article_'s `author` node. Notice that this works because roles are merged.

```markup
<div data-role="article">
  <chtml-import data-role="article-author" data-namespace="html/badge/user"></chtml-import>
</div>
```

The final composition gives us:

```markup
<div data-role="article">
  <div data-role="article-author user" data-namespace="html/badge/user" style="color:black">
    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>
  </div>
</div>
```

**Other Attributes:** Any other attribute found on an import element \(except the `data-namespace` attribute\) will be _set_ on the imported component if not already exists.

### Directives Recomposition

Any _directives_ found on an import element will be appended to the component's _directives_ block. Potential duplicates can normally be managed with tags. \(Tags are covered later down.\)

This is what happens below.

```markup
<template is="chtml-bundle">

  <div data-role="user" data-namespace="html/badge/user">

    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>

    <script type="text/scoped-js">
      // @tag1
      tree.name.el.append(model.first_name);
    </script>

  </div>

</template>
```

```markup
<div data-role="article">
  <chtml-import data-role="article-author" data-namespace="html/badge/user">

    <script type="text/scoped-js">
      // @tag1 !important
      tree.name.el.append(model.first_name.toUpperCase());
    </script>

  </chtml-import>
</div>
```

The final composition would give us:

```markup
<div data-role="article">
  <div data-role="user" data-namespace="html/badge/user">

    <div data-role="user-avatar"></div>
    <div data-role="user-name"></div>

    <script type="text/scoped-js">
      // @tag1
      tree.name.el.append(model.first_name);
      // @tag1 !important
      tree.name.el.append(model.first_name.toUpperCase());
    </script>

  </div>
</div>
```

Note: Tags are experimental and haven't been officially released

#### Managing Recomposition

As a general rule, everything found on an import element will be stripped and composed into the component it imports. But this recomposition can be controlled. There are two ways.

**Using the Norecompose Attribute:** To object to recomposition on an element, the _norecompose_ attribute can be used. If without a value, this attribute disables recomposition completely. Setting its value to `*` achieves the same thing. A list of attribute names may, however, be set to specify the attributes that should be excluded from composition.

```markup
<template is="chtml-bundle">

  <div data-role="user" data-namespace="html/badge/user" style="color:blue" norecompose="style"></div>
</template>
```

To exclude directives from recomposition, the `@directives` keyword should be added to _norecompose_ list.

```markup
<div data-role="user" data-namespace="html/badge/user" norecompose="style @directives">

  <script type="text/scoped-js">
    tree.name.el.append(model.first_name);
  </script>

</div>
```

The _norecompose_ directive may also be set generally for all modules in a bundle.

```markup
<template is="chtml-bundle" norecompose="style @directives">

  <div data-role="user" data-namespace="component/badge/user" style="color:blue">

    <script type="text/scoped-js">
      tree.name.el.append(model.first_name.toUpperCase());
    </script>

  </div>

</template>
```

With a bundle-level _norecompose_ directive in place, modules will now be imported without having the listed attributes recomposed. A module-level _norecompose_ directive could still be set to list additional module-specific attributes for exclusion.

### Node-To-Node Recomposition

Nodes in a component can be replaced on each import we make of it. We do this by predefining the replacement nodes on the import element ahead of the incoming component. On arrival, the respective nodes on the imported component matched by these replacement nodes will be replaced.

This is what happens below. We import the _user_ component into the _article_ component, but with a replacement node \(`user-name`\) that now features special styling.

```markup
<div data-role="article">
  <div>
    <chtml-import data-role="article-author" data-namespace="html/badge/user">
      <div data-role="user-name" style="text-transform:uppercase; font-weight:bold"></div>
    </chtml-import>
    <div>
      <div data-role="article-description"></div>
    </div>
  </div>
</div>
```

The final composition would give us:

```markup
<div data-role="article">
  <div>
    <div data-role="article-author user" data-namespace="html/badge/user" style="color:black">
      <div data-role="user-avatar"></div>
      <div data-role="user-name" style="text-transform:uppercase; font-weight:bold"></div>
    </div>
    <div>
      <div data-role="article-description"></div>
    </div>
  </div>
</div>
```

Attributes and directives recomposition also happens on each node replacement. Before they are finally replaced, attributes and directives from component nodes are composed into replacement nodes. So replacement nodes inherit properties of the component's original nodes.

Now a few notes apply:

* Replacement nodes must be at the root of the import element if they must be discovered.
* Replacement nodes must define a role that corresponds to an original node in source component if a replacement must take place. Other root-level elements and nodes found on an import will get copied to the root of the imported component.

#### Recursive Recomposition

When we import a component and replace its node, we perform one level of import. But we can use replacement nodes to perform another level of import - a replacement node also being an import on its own.

For example, as we import an _article_ component into a page, we could predefine a replacement node for its `author` node, but this time, the replacement node will be an import on its own, referencing a special type of _user_ component.

This is what happens below. In the second level of import, we're importing a new type of _user_ component as the _article_'s `author` node. This _user_ component splits a user's name as _first name_, and _last name_ \(`fname` and `lname`\).

```markup
<html>
  <head>"</head>
  <body>
    <chtml-import data-role="body-main" data-namespace="html/content/article/readonly">

      <chtml-import data-role="article-author" data-namespace="html/badge/user2"></chtml-import>

    </chtml-import>
  </body>
</html>
The final composition would give us:
<html>
  <head></head>
  <body>
    <div data-role="body-main article" data-namespace="html/content/article/readonly">

      <div data-role="article-author user" data-namespace="html/badge/user2">
        <div data-role="user-avatar"></div>
        <div data-role="user-fname"></div>
        <div data-role="user-lname"></div>
      </div>

    </div>
  </body>
</html>
```

We could go even deeper to a third level, and a fourth, and as far as composition requires!

## Inheritance

In CHTML, any piece of code is considered a body of knowledge. Inheritance provides extra opportunities to avoid costly duplication of knowledge. It's a type of code reuse that is inherent - by virtue of where components live; it requires no extra steps to work.

### Namespace-Based Inheritance

The namespace-based component layout in CHTML is no mere naming convention or code organization. It brings with itself the wonderful concept of inheritance.

In CHTML, components in a namespace are believed to be built off their supernamespace. So, the component at `html/content/article/readonly/dark-mode` is believed to be built off the component at `html/content/article/readonly`, which in itself, is believed to be built off the root component at `html/content/article`. Now instead of having to repeat common semantics down the namespace, CHTML makes components inherit them!

**Components Are Implicitly Composed.** Semantics from the component at a supernamespace are, by default, composed into components at each level down a namespace path.

By default, only attributes and directives composition is performed, which means that nodes are not inherited. Components down the namespace thus retain their unique structure.

Here is how this could look.

```markup
<template is="chtml-bundle">

  <!-- Standard, readonly article -->
  <div data-role="article" data-namespace="html/content/article/readonly">

    <div data-role="article-title"></div>
    <div data-role="article-content"></div>

    <script type="text/scoped-js">
      tree.title.el.append(model.title);
    </script>

  </div>

  <!-- Dark-mode, readonly article -->
  <div data-namespace="html/content/article/readonly/dark-mode">
    <div style="color:white; background-color:black">
      <div data-role="article-title"></div>
      <div data-role="article-content"></div>
    </div>
  </div>

</template>
```

Notice that in our _dark-mode_ edition of an _article_ component, it was not necessary to redefine the role "article". The directives were also inherited.

Inheritance takes effect at the time a component is retreived.

Taking things a little further, a node-to-node recomposition is also possible. Now, structural semantics can be inherited. The `chtml-import` element comes into play here, this time as a component definition on its own. Here is how this looks:

```markup
<template is="chtml-bundle">

  <!-- Dark-mode, readonly article, with a different type of content node  -->
  <chtml-import data-namespace="html/content/article/readonly/dark-mode/framed">

    <div data-role="article-content" style="border-color:white"></div>

  </chtml-import>

</template>
```

Now so much has been saved!

**Namespaces Can Be Used Ahead of Implementation.** By virtue of inheritance, we would be safe to find a component deep in a namespace that is yet to be implemented, as long as implementations exist up the namespace hierarchy.

Right below, we're importing an animated type of _article_ that we're yet to create. Maybe soon; maybe someday! But we can, at the moment, make do with the closest implementation that lives up the namespace hierarchy.

```markup
<!-- file: index.html -->
<html>
  <head>"</head>
  <body>
    <chtml-import data-role="body-main" data-namespace="html/content/article/readonly/dark-mode/animated"></chtml-import>
  </body>
</html>
```

Now this really allows us to progressively build features into an app while using a real layout plan from the start.

### Cross-Bundle Inheritance

When multiple bundles are defined on a document, they are all used in a cascaded manner; in the same way multiple CSS stylesheets work. This helps us reuse code across bundles from multiple sources.

When a namespace is accessed for import, all bundles are queried for the requested component and matches are gathered and composed. Components from latter bundles are made to inherit components from earlier bundles.

In the example below, we assume that the first bundle was linked from a third-party. The second bundle is a local bundle that inherits a component from the first bundle and applies dark-mode styling.

```markup
<template is="chtml-bundle">

  <!-- Standard, readonly article -->
  <div data-role="article" data-namespace="html/content/article/readonly">
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
  </div>

  <img src="/assets/img/ui/banner.png" data-namespace="ui/banner" />

</template>
<template is="chtml-bundle">

  <!-- Dark-mode, readonly article -->
  <chtml-import data-namespace="html/content/article/readonly/dark-mode" style="color:white; background-color:black"></chtml-import>

  <img src="/assets/img/brand/logo.png" data-namespace="img/brand/logo" />

</template>
```

The two bundled images above do not live in the same namespace. So they are imported independently of each other.

But how does this work with the namespace-based inheritance? The answer lies in a wonderful type of algorithm.

### The Inheritance Matrix

With namespace-based inheritance working from left to right and cross-bundle inheritance working top-down, a matrix is formed. On this matrix, inheritance begins from the left and moves level-by-level towards the right, but with top-down inheritance being applied at each level. This becomes clear in an example.

The two bundles below each have a pair of components. Each pair constitutes namespace-based, left-right inheritance. The two bundles themselves constitute cross-bundle, top-down inheritance. Now we have four components and three have a CSS color each. Let's see what happens as we try to import them.

```markup
<template is="chtml-bundle">

  <div style="color:blue" data-namespace="html/base1/base2"></div>
  <div style="color:red" data-namespace="html/base1/base2/red"></div>

</template>

<template is="chtml-bundle">

  <div style="color:yellow" data-namespace="html/base1/base2"></div>
  <div data-namespace="html/base1/base2/red"></div>

</template>
```

If we tried importing `html/base1/base2`, all bundles will first be asked for html/base1 to see if there is anything for the following level to inherit. The first bundled is asked first, then the second. Since no component can be found, we come empty to `html/base1/base2`. Again the first bundle is asked first, and we find a blue component, then the second bundle, and we find a yellow component. They are composed to produce a yellow component, which is what we would expect.

```markup
<div style="color:blue; color:yellow" data-namespace="html/base1/base2"></div>
```

If we tried importing `html/base1/base2/red`, the same process above is follwed and by the time we get to `components/base1/base2`, we get a yellow component, as before. This brings us to `html/base1/base2/red` with two colors already inherited. Here again, the first bundle is asked first, and we find a red component, then the second bundle, and we find the final component but without a color. They get composed to produce a red component, which is what we would expect.

```markup
<div style="color:blue; color:yellow; color:red" data-namespace="html/base1/base2/red"></div>
```

Obviously, the order of these bundles matter.

## Cascading - \(Experimental Feature\)

The level of reusability and composition in CHTML requires being able to manage \(accept or reject\) duplicate directives. This is especially important as multiple bundles from different vendors may now be connected to the same document. Happily, the concept of cascading enables us to do so.

### Cascading Rules

The order in which these directives are read in CHTML is governed by the following rules; they're for the most part, like CSS cascading rules.

**Unique and Duplicate Declarations.** Declarations of the same tag are duplicates. Each declaration overrides a previous declaration.

The following directives, for example, are duplicates. The last one is what takes effect.

```markup
<body data-role="app">

    <script type="text/scoped-js">
      // @tag1
      append('Hello World!');
      // @tag1
      append('Thanks for visiting!');
    </script>

</body>
```

**The !important keyword.** As with CSS, the `!important` keyword can be used with a tagname. It marks the declaration as of high priority and asks to replace other declarations of the same tag, whether found before or after.

The first directive below takes precedence; and the second, ignored - overridden.

```markup
<body data-role="app">

    <script type="text/scoped-js">
      // @tag1 !important
      append('Hello World!');
      // @tag1
      append('Thanks for visiting!');
    </script>

</body>
```

Now where two declarations of the same tag both have the `!important` keyword, they are treated as duplicates; the latter overrides the former. This is what happens below.

```markup
<body data-role="app">

    <script type="text/scoped-js">
      // @tag1 !important
      append('Hello World!');
      // @tag1 !important
      append('Thanks for visiting!');
    </script>

</body>
```

**The !fallback keyword.** For further control, the `!fallback` keyword can also be used with a tag. It marks the declaration as of lower priority and asks to be used ONLY where no declarations of the same tag can be found anywhere before or after.

```markup
<body data-role="app">

    <script type="text/scoped-js">
      // @tag1
      append('Hello World!');
      // @tag1 !fallback
      append('Thanks for visiting!');
    </script>

</body>
```

This is especially useful where newer declarations do not want to take advantage of their position and want to relinquish their rights first to any previous declarations.

Now where two declarations of the same tag both have the `!fallback` keyword, they a treated as duplicates; the latter overrides the former. This is what happens below.

```markup
<body data-role="app">

    <script type="text/scoped-js">
      // @tag1 !fallback
      append('Hello World!');
      // @tag1 !fallback
      append('Thanks for visiting!');
    </script>

</body>
```

