# The Component Model

A component is simply an element that has been assigned a specific role. Certain elements in its subtree - called nodes - may also be associated with this role to form a special role/node relationship. This relationship is the CHTML component model.

Component models are formed at markup level, either explicitly or implicitly.

## Explicit Models

Explicit component role/node relationships are formed using the `data-role` attribute.

```markup
<div data-role=”article”></div>
```

The `data-role` attribute is also used to associate nodes. This time, the subrole is prefixed with the owning role.

```markup
<div data-role=”article”>
  <div data-role=”article-author”></div>
  <div data-role=”article-content”></div>
</div>
```

And nodes could be anywhere within the component’s subtree.

```markup
<div data-role=”article”>
  <div>
    <div data-role=”article-author”></div>
    <div>
      <div data-role=”article-content”></div>
    </div>
  </div>
</div>
```

So, regardless of a component’s DOM structure, we can always have a clear component model to bank on.

```markup
article
    |
    |- author
    |
    |- content
```

### Component Hint

The nodes implemented by a component may optionally be listed in a separate attribute to give everyone a quick hint as to what a component is made of. The `data-tree` attribute is used for this purpose.

```markup
<div data-role=”article” data-tree="author content">
  <div>
    <div data-role=”article-author”></div>
    <div>
      <div data-role=”article-content”></div>
    </div>
  </div>
</div>
```

### Nested Components

Components can be nested. So a node may constitute a component of its own, and establish its own role and have its own nodes.

Below, the `article-author` _node_ is also a _component_.

```markup
<div data-role=”article”>
  <div>
    <div data-role=”article-author user”>
      <div data-role=”user-avatar”></div>
      <div data-role=”user-name”></div>
    </div>
    <div>
      <div data-role=”article-content”></div>
    </div>
  </div>
</div>
```

This produces the following component model.

```markup
article
    |
    |- author (user)
    |    |
    |    |- avatar
    |    |
    |    |- name
    |
    |- content
```

Where two identical roles \(component scopes\) are nested, nodes are associated with the role that is closest to them up the hierarchy.

```markup
<div data-role=”article anotherrole”>
  <div>
    <div data-role=”article-author”></div>
    <div data-role=”article-content”></div>
    <div data-role=”article-brief article”>
      <div data-role=”article-date”></div>
      <div data-role=”article-description”></div>
      <div data-role=”anotherrole-othernode”></div>
    </div>
  </div>
</div>
```

The nested articles produce the following component model.

```markup
Article (anotherrole)
    |
    |- author
    |
    |- content
    |
    |- othernode
    |
    |- brief (article)
        |
        |- date
        |
        |- description
```

## Implicit Models

Certain elements in HTML have been naturally designed to work together in a predefined manner. These natural relationships, or implicit models, are automatically recognized in CHTML and need not be explicitly designed.

Not commonly known, every element has a design note or specification that specifies its **category** and dictates what and what can serve as its content – better known as the element’s **content model**. For example, the `<html>` element must only have the `<head>` and `<body>` tags as its direct children. Okay, that’s common knowledge, but really, that’s the spec dictating.

Content models are implicit component models in CHTML; here an element’s category, if defined, represents the role, and its permissible elements, serve as nodes. Below are examples of these implicit models. \(The same rule holds for other models as defined in the [HTML standard](https://html.spec.whatwg.org/multipage).\)

The `<html>` element’s content model permits two elements that must also be direct children.

```markup
<html>
  <head></head>
  <body></body>
</html>
```

This gives us the component model:

```markup
html
    |
    |- head
    |
    |- body
```

The `<head>` element’s content model permits only elements that have been categorized a Metadata elements.

Below, the following Metadata elements can only be used once in the `<head>` element.

```markup
<head>
  <title></title>
  <base />
</head>
```

This gives us the following component model

```markup
head
    |
    |- title
    |
    |- base
```

Now certain elements are permitted to appear any number of times within their permissible scope. The `<head>` element, again, permits multiple `<meta>` elements.

```markup
<head>
  <title></title>
  <meta name=”” content=”” />
  <meta name=”” content=”” />
  <meta name=”” content=”” />
  <base />
</head>
```

This gives us the following component model. \(Notice that the meta node now becomes a node list.\)

```markup
head
    |
    |- title
    |
    |- meta (3)
    |
    |- base
```

### Nested Elements Create Scope Boundaries

Where two identical scopes are nested, nodes are associated with the scope that is closest to them up the hierarchy. The `<body>` element and the `<blockquote>` element are a good example of nested identical scopes.

The `<body>` element is categorized as Sectioning Root which permits elements categorized as Flow Content. The `<blockquote>` element is one of those Flow Content elements. \(Most elements are categorized as Flow Content – `h1`, `div`, `p`, etc.\) Now the `<blockquote>` element is also a Sectioning Root that could have its own Flow Content. So below, every element is a node to the body’s Sectioning Root scope except those within blockquote’s Sectioning Root scope.

```markup
<body>
  <h1></h1>
  <div>
    <p></p>
  </div>
  <blockquote>
    <h1></h1>
    <div></div>
  </blockquote>
  <div></div>
</body>
```

This gives us the following component model.

```markup
body
    |
    |- div (2)
    |    |
    |    |- [0]
    |    |    |- p (1)
    |    |
    |    |- [1]
    |
    |- h1 (1)
    |
    |- blockquote (1)
    |    |
    |    |- [0]
    |        |
    |        |- h1 (1)
    |        |
    |        |- div (1)
    |
    |- p (1)
```

Another form of the Sectioning Root category is Sectioning Content. Elements in this category are `<article>`, `<aside>`, `<nav>`, and `<section>`. Notice how the `<header>` element, being a Flow Content element, is semantically associated below. Also note that there cannot be more than one within its permissible scope.

```markup
<body>
  <header></header>
  <article>
    <header></header>
    <div></div>
  </article>
</body>
```

This gives us the following component model.

```markup
body
    |
    |- header
    |
    |- article (1)
        |
        |-[0]
            |
            |- header
            |
            |- div (1)
```

### ARIA Roles Are Automatically Recognized

The implicit and explicit roles defined in the ARIA specification are automatically recognized in CHTML. Elements that have an explicit or implicit ARIA role can be accessed by both their rolename and their tagname.

```markup
<body>
  <div role=”header”></div>
  <article>
    <div role=”header”></div>
    <div></div>
  </article>
</body>
```

This gives us the following component model.

```markup
body
    |
    |- div (1)
    |
    |- header
    |
    |- article (1)
        |
        |- [0]
            |
            |- div (1)
            |
            |- header
```

The `<section>` element has the implicit ARIA role region. So both the names section and region will point to the `<section>` element.

```markup
<body>
  <section>
  </section>
  <section>
  </section>
</body>
```

This gives us the following component model.

```markup
body
    |
    |- section/region (2)
```

The `<ul>` element has the implicit ARIA role list. The `<li>` element has the implicit ARIA role listitem. Here’s how it looks.

```markup
<body>
  <ul>
    <li></li>
    <li></li>
  </ul>
</body>
```

This gives us the following component model. Also notice that the `<li>` element do not reflect in the `<body>` as their permissible scope ends with the `<ul>`.

```markup
body
    |
    |- ul/list (1)
        |
        |- [0]
            |
            |- li/listitem (2)
```

