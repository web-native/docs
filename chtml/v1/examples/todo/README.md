## A TODO List Example

Below is a TODO list composed from a JavaScript array using Scoped HTML, Scoped JS in combination with the HTML Partials API.

```html
<html>

    <head>

        <template name="template1">
            <li partials-slot="item">
                <script type="scoped">this.innerHTML = text;</script>
            </li>
        </template>

    </head>

    <body>

        <div root id="todo" template="template1">

            <h2 scoped:id="title"></h2>

            <ul scoped:id="items"></ul>

            <script type="scoped">
                this.scopeTree.title.innerHTML = title;
                items.forEach(itemBinding => {
                    let itemElement = this.template.partials.item[0].cloneNode(true);
                    itemElement.bind(itemBinding);
                    this.scopeTree.items.append(itemElement);
                });
            </script>

        </div>

        <script>
            document.querySelector('#todo').bind({
                title: 'My TODOs',
                items: [
                    {text: 'TODO-1'},
                    {text: 'TODO-2'},
                    {text: 'TODO-3'},
                ],
            });
        </script>
    </body>

</html>
```

We could even add the ability to add/remove items. For the *remove* feature, we'd add a *click* event listener to the item element definition. For the *add* feature, we'd add a button to the TODO container that calls the `add()` method of the TODO application. 

```html
<html>

    <head>

        <template name="template1">
            <li root partials-slot="item">
                <span scoped:id="content"></span>
                <button scoped:id="remover">Remove</button>
                <script type="scoped">
                    this.scopeTree.content.innerHTML = text;
                    this.scopeTree.remover.addEventListener('click', () => this.remove());
                </script>
            </li>
        </template>

    </head>

    <body>

        <div root id="todo" template="template1">

            <h2 scoped:id="title"></h2>

            <ul scoped:id="items"></ul>

            <button scoped:id="adder">Add</button>

            <script type="scoped">
                this.scopeTree.title.innerHTML = title;
                items.forEach(itemBinding => {
                    let itemElement = this.partials.item[0].cloneNode(true);
                    this.scopeTree.items.append(itemElement.bind(itemBinding));
                });
                this.scopeTree.adder.addEventListener('click', () => addItem());
            </script>

        </div>

        <script src="//unpkg.com/@web-native-js/observer/dist/main.js"></script>
        <script>
            let Obs = window.WebNative.Observer;
            let count = 4;
            document.querySelector('#todo').bind({
                title: 'My TODOs',
                items: [
                    {text: 'TODO-1'},
                    {text: 'TODO-2'},
                    {text: 'TODO-3'},
                ],
                addItem() {
                    Obs.get(this.items, 'push')({text: 'TODO-' + count ++});
                },
            });
        </script>
    </body>

</html>
```
