## A Tooling Example

That CHTML is a foundational technology just gives us every room to bring our own tooling. This example shows how we could make a DOM abstraction API, like jQuery, available to scoped scripts.

Below, we're simply binding the `$` variable globally for use in every scoped script.

```html
<body>

    <div root id="alert">
        <div scoped:id="message"></div>
        <script type="scoped">
            $(this.scopeTree.message).html(message);
        </script>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script>
        document.bind({$: window.jQuery});
        document.querySelector('#alert').bind({
            message: 'This task is now complete!',
        });
    </script>

</body>
```

Tooling can also save the day on the efficiency of DOM manipulation. Generally, surgically updating the DOM may have performance implications on the UI, as arising from layout thrashing (see [this article](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) on Web Fundamentals). But we also don't need as much as a *Virtual DOM*. A technique like that of [fast DOM](https://github.com/wilsonpage/fastdom) could just suffice. The upcoming [PlayUI](https://docs.web-native.dev/play-ui) library has a design that brings this technique (what we like to call *async DOM*) in the syntax-sugar of jQuery.

```html
<body>

    <div root id="alert">
        <div scoped:id="message"></div>
        <script type="scoped">
            $(this.scopeTree.message).html(message).then(() => {
                // Do something sync
            });
        </script>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="//unpkg.com/@web-native-js/play-ui/dist/main.js"></script>
    <script>
        document.bind({$: window.WebNative.PlayUI});
        document.querySelector('#alert').bind({
            message: 'This task is now complete!',
        });
    </script>

</body>
```