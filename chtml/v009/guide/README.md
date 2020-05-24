# CHTML Guide

## Introduction to CHTML

CHTML is an adaptable system of specifications and tooling that brings UI component architecture to the HTML language itself. 

**Problem is:** you cannot encapsulate subtrees in a HTML document. Every element in a HTML document just happens to be associated with the global scope of the document's scripts and styles.

![Everything is connected to a global scope](/.gitbook/assets/chtml.fw.png)

The above leads to specificity wars in CSS selectors. -`body > div:first-child {}` - and other naming mechanism that quickly becomes very difficult to maintain - `querySelectorAll('#first-div')`.

**What we want is:** the ability to imply encapsulated subtrees that are functionally independent. They should also be able to bundle their own CSS and JavaScript.

![Subtrees that establish their own scope for CSS and JavaScript](/.gitbook/assets/chtml-2.fw.png)

The above represents a document composed of functionally-independent blocks that might have been **defined statically** or dropped into their current locations **dynamically**.

What CHTML is making possible is simply a standard way to define these independent scopes of functionality in a way that makes everything automatically fit together. Whatever goes into these scopes - the content of the subtrees, their scoped-CSS and scoped-JS - remains all up to you. As we will now see, this is going to cut much of the long story short in your design process. It's a _functional design system_ built to facilitate your own design system!

### Architectural Concepts

A functionally-independent block in the UI is called a component. They can be statically-defined right within the `<body>` of the document. Or they can be defined outside of `<body>` and dynamically imported into the `<body>` area anywhere, anytime - as we will see shortly.

Every element is potentially a component; no ceremonies involved!

```markup
<body>
  <div>I am potentially a component!</div>
</body>
```

But we can take things further by assigning structural parts - called nodes - to this component using a role-based markup pattern.

Below, we've chosen to use the `data-role` attribute for this purpose.

```markup
<body>

  <div data-role="user">
    <div data-role="user-avatar"></div>
    <div>
      <div data-role="user-name"></div>
      <div data-role="user-title"></div>
    </div>
  </div>
  
</body>
```

This gives us a functional component in the form of a model!

```text
user
  |-- avatar
  |-- name
  |-- title
```

This component model is going to be the basis for anyone or any script/bot to interface with our component.

Now whatever later becomes of this component wouldn't be anyone's headache!

![](/.gitbook/assets/chtml-3.fw.png)

#### Nesting Components

A component's node can be a component of its own.

Below, our _user_ component is now the _author_ node of an _article_ component.

```markup
<body>

  <div data-role="article">
  
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
    
    <div data-role="article-author user">
      <div data-role="user-avatar"></div>
      <div>
        <div data-role="user-name"></div>
        <div data-role="user-title"></div>
      </div>
    </div>
    
  </div>
  
</body>
```

Our component model now looks like this.

```markup
article
  |-- title
  |-- content
  |-- author (user)
      |-- avatar
      |-- name
      |-- title
```

#### Reusing Components

Reusable components are defined in a `<template>` element. They are assigned a _namespace_ for unique identification.

Below, we've chosen the `data-namespace` attribute for namespacing.

```markup
<head>
  <template>
    <div data-namespace="component/first">Import me as many times as you need me!</div>
    <div data-namespace="component/second" data-role="card">...</div>
  </template>
</head>
```

We can now place a component anywhere in the `<body>` area  using their namespace. We'll be going with the `<chtml-import>` element for importing.

```markup
<html>
<head>

  <template is="chtml-bundle">
    <div data-namespace="component/first">Import me as many times as you need me!</div>
    <div data-namespace="component/article" data-role="article">...</div>
  </template>
  
</head>
<body>

  <chtml-import data-namespace="component/article"></chtml-import>
  
</body>
</html>
```

### Behavioural Concepts

In CHTML, components can encapsulate their own behaviour using scoped-JS; and this is unique to CHTML. A scoped-script goes by a custom `type` attribute. This is needed to prevent the browser from running the script; CHTML is coming to pick it up.

Below, we've decided to use `text/scoped-js` as our script's type attribute.

```markup
<body>
  <div data-role="article">
  
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
    
    <script type="text/scoped-js">
      // DOM manipulation goes here
    </script>
    
  </div>
</body>
```

A scoped script must be at the root of its owning component. It is used solely for DOM manipulation and other _presentational logic._ It is limited to the scope of its owning component and can reference anything within this scope - that is, properties of the component object.

The `el` property, for example, serves as a reference the component's underlying DOM element. Now we can reference `el` in ourscript and work with its entire API.

```markup
<body>
  <div data-role="article">
  
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
    
    <script type="text/scoped-js">
    
      // Append content to the article's root element
      el.append('Thanks for reading!');
      
      // Blink the article on click
      el.addEventListener('click', () => {
        el.animate([{opacity:0}, {opacity:1}], {duration:400});
      });
      
    </script>
    
  </div>
</body>
```

The `tree` property,  as another example, represents the component's tree of nodes. Here we can reference nodes by name and access their own underlying DOM elements.

```markup
<body>
  <div data-role="article">
  
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
    
    <script type="text/scoped-js">
    
      // What we want now is to blink the article
      // on clicking its author node
      tree.author.el.addEventListener('click', () => {
        el.animate([{opacity:0}, {opacity:1}], {duration:400});
      });
      
    </script>
    
  </div>
</body>
```

#### Data Binding

An application can use a component's `bind()` method to bind values or other objects to the component. The `bind()` method simply assigns this value or object to the component's `binding` property. We can now pick this input data up and choose how and where we display it on the component.

Here's how our article component would handle the data it receives:

```markup
<body>
  <div data-role="article">
  
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
    
    <script type="text/scoped-js">
    
      if (binding) {
        tree.title.el.innerHTML = binding.title;
        tree.content.el.innerHTML = binding.content;
      }
      
    </script>
    
  </div>
</body>
```

Let's put this in a complete example showing how the data comes in the first place and how nested components receive their own data.

```markup
<html>
<head>

  <script type="text/javascript">
    // Our code should run when
    // the document is ready
    Chtml.ready(() => {
      var articleComponent = Chtml.from('#rootComponent');
      
      var data = {
        title: 'Hello World!',
        content: 'Is the pandemic over yet?',
        author: {
          name: 'John Doe',
          title: 'Writer',
        }
      };
      
      articleComponent.bind(data);
    });
  </script>
  
</head>
<body>

  <div data-role="article" id="rootComponent">
  
    <div data-role="article-title"></div>
    <div data-role="article-content"></div>
    
    <script type="text/scoped-js">
      if (binding) {
        tree.title.el.innerHTML = binding.title;
        tree.content.el.innerHTML = binding.content;
        // We will mow also bind() binding.author
        // to the author subcomponent
        tree.author.bind(binding.author);
      }
    </script>
    
    ---------------------------
    <div data-role="article-author user" style="font-size: smaller">
      <div data-role="user-avatar"></div>
      <div>
        <div data-role="user-name"></div>
        <div data-role="user-title"></div>
      </div>
      <script type="text/scoped-js">
        if (binding) {
          tree.name.el.innerHTML = binding.name;
          tree.title.el.innerHTML = binding.title;

        }
      </script>
    </div>
    ----------------------------->
    
  </div>
  
</body>
</html>
```

Above, our application is the code that is running in the document's `<head>` section. Also take particular note of the flow of data. Here, the article component receives data via its `binding` property, and also forwards a branch of this data to its child component using the child's `bind()` method. The child component receives this _data branch_  via its own `binding` property and does its own thing.

And _tada_, we have a full-fledged app up there!

But it also leaves us with much to improve on. First, instead of statically defining the article's _author_ node, we could dynamically import it into the article. Being out of sight would simplify how we think of our code. We will apply this in our final code.

### Reactivity

While we rejoice over our first app, it is pretty static! We have to have a way to update the displayed data when it gets updated in the application. CHTML saves us the stress of manually tracking changes by defining "reactivity" as part of the system. This holds that each of the root-level statements in a scoped script is re-executed when any of the statement's references gets updated in the application.

Let's play with reactivity! In our app above, let's do something that will change the article's author to someone else. Our component system should automatically pick this change up and re-render the appropriate fragment in the UI.

```markup
<head>

  <script type="text/javascript">
    // Our code should run when
    // the document is ready
    Chtml.ready(() => {
      var articleComponent = Chtml.from('#rootComponent');
      
      var data = {
        title: 'Hello World!',
        content: 'Is the pandemic over yet?',
        author: {
          name: 'John Doe',
          title: 'Writer',
        }
      };
      
      articleComponent.bind(data);
      
      // The updated author...
      setTimeout(() => {
        Reflex.set(data, 'author', {
          name: 'Another John',
          title: 'Designer',
        });
      }, 1000);
    });
  </script>
  
</head>
```

The article's author should now change after `1000ms`. Notice that we're using `Reflex.set()` for this; CHTML is based on _reflex actions_ - a special way of publishing and observing changes on any type of object or array. It saves us the pain of employing heavy `Observable` classes or some other reactivity machines that make life difficult.

Now that's it - our first CHTML-based app!

### Conclusion

Congratulations on coming this far! You've done something very uncommon: building a dynamic application in plain HTML and vanilla JS! We hope you want to find out more. Here's a summary of what we've done:

* Employed semantic markup using the role-based markup pattern.
* Defined and imported reusable components.
* Embedded scoped-JS within components.
* Traversed and manipulated the component tree from within scoped-JS.
* Received data from an application; passed a branch of received data to a child component.
* Triggered an update in the UI with a change in the application's state.

But what about CHTML's template syntax, template compilers and other build tools? Short answer: we don't have those here! Long answer: these are only a means to an end for the path taken by other UI frameworks!

If you come from a UI framework, like Angular, Vue.js et all, that follows a JS-based approach to UI development, you are about to experience UI development as a field of design than engineering! Take a step further to see how CHTML helps you build elegant, more empowering user interfaces!

If you come from a jQuery background, welcome home. You can have the same short, terse syntax you're used to within scoped-JS. So instead of saying `tree.name.el.innerHTML = binding.name`, you could be saying `tree.name.html(binding.name)`. The reason CHTML ships in vanilla HTML/JS is to provide everyone a clean slate to scaffold their choice of API! The rest of the guides will take you!

We'll now conclude with a complete HTML code you can take away to try. It is in the format that puts all our current techniques to use. We've also finally embedded `Chtml` library in the `<head>` section with a script tag. So you can simply copy and paste this code in a blank HTML file and render.

```markup
<html>
<head>
  
  <title>Hello - by CHTML</title>
  <script src="https://unpkg.com/@web-native-js/chtml/dist/main.js"></script>
  <script src="https://unpkg.com/@web-native-js/reflex/dist/main.js"></script>
  
  <script type="text/javascript">
  
    const Chtml = window.WebNative.Chtml;
    const Reflex = window.WebNative.Reflex;
    
    // Our code should run when
    // the document is ready
    Chtml.ready(() => {
      var articleComponent = Chtml.from('#rootComponent');
      
      var data = {
        title: 'Hello World!',
        content: 'Is the pandemic over yet?',
        author: {
          name: 'John Doe',
          title: 'Writer',
        }
      };
      
      articleComponent.bind(data);
      
      // The updated author...
      setTimeout(() => {
        Reflex.set(data, 'author', {
          name: 'Another John',
          title: 'Designer',
        });
      }, 6000);
    });
  </script>
  
  <template is="chtml-bundle">
    
    <!-- USER -->
    <div data-namespace="component/user" data-role="user">
      <div data-role="user-avatar"></div>
      <div>
        <div data-role="user-name"></div>
        <div data-role="user-title"></div>
      </div>
      <script type="text/scoped-js">
        if (binding) {
          tree.name.el.innerHTML = binding.name;
          tree.title.el.innerHTML = binding.title;

        }
      </script>
    </div>
    
    <!-- ARTICLE (uses USER) -->
    <div data-namespace="component/article" data-role="article">
  
      ---------------------------
      <div data-role="article-title"></div>
      <div data-role="article-content"></div>
      ---------------------------
      <chtml-import data-role="article-author" data-namespace="component/user" style="font-size: smaller"></chtml-import>
      
      <script type="text/scoped-js">
        if (binding) {
          tree.title.el.innerHTML = binding.title;
          tree.content.el.innerHTML = binding.content;
          // We will mow also bind() binding.author
          // to the author subcomponent
          tree.author.bind(binding.author);
        }
      </script>
      
    </div>
    
  </template>
  
</head>
<body>

  <chtml-import data-namespace="component/article" id="rootComponent"></chtml-import>
  
</body>
</html>
```

## Next Steps

Visit the following guides then start writing Component-Oriented HTML.

* [Strucutural Concepts](structural-concepts/)
* [Behavioural Concepts](behavioural-concepts/)
* [Compositional Concepts](compositional-concepts/)

You may also check out.

* [Installation Guide](installation.md)
* [Additional Guides](extras/)

