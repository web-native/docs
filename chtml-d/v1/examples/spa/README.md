## A Single Page Application Example

This example makes an SPA of *templates and slots* composition. Below, we're using the two template elements to each represent a route - each holding partials that are unique to a route. Then we point the body element to implement the template whose namespace matches the current URL.

```html
<html>

    <head>

        <template namespace="route/home">
            <h1 partials-slot="headline">
                Welcome Home!
            </h1>
            <p partials-slot="content">
                <a href="#/about">About Me</a>
            </p>
        </template>

        <template namespace="route/about">
            <h1 partials-slot="headline">
                About Me!
            </h1>
            <p partials-slot="content">
                <a href="#/home">Back to Home</a>
            </p>
        </template>

    </head>

    <body template="route/home">

        <header></header>

        <main>
            <div id="banner">
                <partials-slot name="headline">404</partials-slot>
            </div>
            <div>
                <partials-slot name="content">Page not Found!</partials-slot>
            </div>
        </main>

        <footer></footer>

        <script>
            window.addEventListener('popstate', e => {
                let path = document.location.hash.substr(1);
                document.body.setAttribute('template', 'route' + path);
            });
        </script>
    </body>

</html>
```

Navigate to a route that does not begin with `#/home` or `#/about`, you should see the default content showing *404*.