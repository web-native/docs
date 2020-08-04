# Scoped JS

Scoped JS is a DOM feature that lets us implement `<script>` elements that are scoped their containing element and completely out of the global browser scope.

## On this page:
+ [Scoped Scripts](#scoped-scripts)
+ [Selective Execution](#selective-execution)
+ [Globals](#globals)
+ [Runtime](#runtime)
+ [Error Handling](#error-handling)
+ [Isomorphic Rendering](#isomorphic-rendering)

## Scoped Scripts

 Scoped scripts have their `this` variable implicitly bound to their containing element. They are defined with the `scoped` MIME type.

```html
<div>
    <div class="message">This task is now complete!</div>
    <div class="exit" title="Close this message.">X</div>
    <script type="scoped">
        this.querySelector('.exit').addEventListener('click', () => {
            this.remove();
        });
    </script>
</div>
```

Other variables in a scoped script are to be explicitly-bound from external values. Variables are bound by name, as in the `message` variable below. 

```html
<body>

    <div id="alert">
        <div class="message"></div>
        <div class="exit" title="Close this message.">X</div>
        <script type="scoped">
            let messageEl = this.querySelector('.message');
            messageEl.innerHTML = message;
        </script>
    </div>

    <script>
        let alertEl = document.querySelector('#alert');
        alertEl.bind({
            message: 'This task is now complete!',
        });
    </script>

</body>
```

Scoped JS gives us this special ability to bring DOM manipulation logic closer to their targets and away from an application. It has *presentational logic* as its sole responsibility and thus helps us keep the main application layer void of the implementation details of the UI. As shown above, an application simply binds its hard-earned values and is done!

## Selective Execution

Scoped JS follows the normal top-down execution of a script. Calling the `.bind()` method with different variable-bindings reruns the script top-down. But as a UI binding langauge, it also features *Selective Execution* where an update to a variable gets to rerun only the corresponding statements within the script - skipping the other statements. This makes for the most-efficient way to keep a block of the UI in sync with little updates from an application. 

To update a variable or multiple variables, call `.bind()` with `false` as a second paremeter.

```js
alertEl.bind({
    variable2: 'New value',
    variable5: 'New value',
}, false);
```

Also, Scoped JS exposes a new DOM property `.bindings` for selectively updating an element's bindings.

```js
alertEl.bindings.variable5 = 'New value',
```

This is illustrated in the clock below.

```html
<body>

    <div id="clock">
        <div class="greeting"></div>
        <div class="current-time"></div>
        <script type="scoped">
            this.querySelector('.greeting').innerHTML = greeting;
            this.querySelector('.current-time').innerHTML = currentTime;
        </script>
    </div>

    <script>
        let clockEl = document.querySelector('#clock');
        clockEl.bind({
            greeting: 'Good Afternoon!',
            currentTime: '00:00:00',
        });

        // Clock ticks
        setInterval(() => {
            clockEl.bindings.currentTime = (new Date).toLocaleString();
        }, 100);
    </script>

</body>
```

Scoped JS also supports the [Observer API](https://docs.web-native.dev/observer) for object observability. With Observer, Scoped JS is able to respond to mutations on the bound data object. So the clock above could be driven by direct updates to the data object.

```html
<script>
    let clockState = {
        greeting: 'Good Afternoon!',
        currentTime: '00:00:00',
    };
    document.querySelector('#clock').bind(clockState);

    // Clock ticks
    setInterval(() => {
        Obs.set(clockState, 'currentTime', (new Date).toLocaleString());
    }, 100);
</script>
```

Scoped JS is also able to pick up deep mutations for statements that reference deep into an object, as in `clock.currentTime`.

```html
<body>

    <div id="clock">
        <div class="greeting"></div>
        <div class="current-time"></div>
        <script type="scoped">
            this.querySelector('.greeting').innerHTML = clock.greeting;
            this.querySelector('.current-time').innerHTML = clock.currentTime;
        </script>
    </div>

    <script>
        let state = {
            clock: {
                greeting: 'Good Afternoon!',
                currentTime: '00:00:00',
            },
        };
        document.querySelector('#clock').bind(state);

        // Clock ticks
        setTimeout(() => {
            Obs.set(state.clock, 'currentTime', (new Date).toLocaleString());
        }, 100);
    </script>

</body>
```

Within the script, the dependency chain is followed even when broken into local variables. Below, a change to `clock.currentTime` will still propagate through `variable1` and  `variable2`. (The first and last statements in the script are left untouched touched, as expected.)

```html
<body>

    <div id="clock">
        <div class="greeting"></div>
        <div class="current-time"></div>
        <script type="scoped">
            this.querySelector('.greeting').innerHTML = clock.greeting;
            let variable1 = clock.currentTime;
            let variable2 = variable1;
            this.querySelector('.current-time').innerHTML = variable2;
            this.style.color = 'blue';
        </script>
    </div>

</body>
```

## Globals

By default, scoped scripts have no access to anything besides what has been explicitly bound into the scope. But they also have an idea of a global scope, seen by every scoped script. This global scope is created by binding on the `document` object itself, using a new `document.bind()` method.

```js
document.bind({
    greeting: 'Good Afternoon!',
});
```

There is also the `document.bindings` property for selectively updating *globals*.

```js
document.bindings.greeting = 'Good Evening!';
```

## Runtime

By design, Scoped JS parses scoped scripts immediately they land on the DOM, but runs them only after the global scope has been initialized with `document.bind()` or the `document.bindings` property. Newer scipts are run immediately after this global runtime initilization. But the runtime of an individual script will begin before the global one on calling the element's `.bind()` method or assigning to its `.bindings` property, or by setting the `autorun` *Boolean* attribute on the script element.

Also, an element may receive bindings before its scoped script is appended or is ready to run. The element's runtime begins the first time both are available.

## Error Handling

Scoped JS features a way to handle syntax or reference errors that may occur with scoped scripts. Normally, these are thrown as exceptions. But they can be silently ignored by setting a directive on the CHTML META tag. Induvidual scripts may also be given a directive, to override whatever the global directive is.

```html
<html>
    <head>
        <meta name="chtml" content="script-errors=0;" />
    </head>
    <body>
        <h1></h1>
        <script type="scoped" error="1">
            this.querySelectorSelectorSelector('h1').innerHTML = headline;
        </script>
    </body>
</html>
```

\* The trailing semi-colon (;) in the CHTML META tag is optional.

## Isomorphic Rendering

The script tag of a scoped script is not always needed for the lifetime of the page. They are discarded by default after parsing. But when a page is rendered on the server and has to be *hydrated* by the browser, it becomes necessary to retain these scripts for revival on the browser. This feature is designed to be explicitly turned on with a directive on the CHTML META tag.

```html
<html>
    <head>
        <meta name="chtml" content="isomorphic=true;" />
    </head>
    <body>
        <h1></h1>
        <script type="scoped">
            this.querySelector('h1').innerHTML = headline;
        </script>
    </body>
</html>
```

Now, running the code `document.bind({headline: 'Hello World'})` both on the server and on the browser should give us the same result.

\* The trailing semi-colon (;) in the CHTML META tag is optional.

**Environment-Specific Bindings**

Sometimes, we want certain bindings to apply only on the server; sometimes, only on the browser. For example, animation is only a thing in the browser. This is the perfect use-case for conditionals.

```html
<div>
    <script type="scoped">
        if (condition) {
            this.animate(...);
        }
    </script>
</div>
```

We would only need to set the appropriate global variable about the current environment: `document.bind({env:'server', headline: 'Hello World'})`.

```html
<div>
    <script type="scoped">
        if (env !== 'server') {
            this.anumate([
                {color:'red'},
                {color:'blue'},
            ], {duration:600,});
        }
    </script>
</div>
```