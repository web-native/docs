# Custom Events

PlayUI's event system can be hooked into to provide custom event implementations. For example, its _tripletap_, \(doubletap\), and _singletap_ gestures are custom implementations/variations of the _tap_ gesture. But custom events can also be just an alias for another event. PlayUI makes everything easy!

To create a custom event, import the `CustomEvents` object.

```javascript
import CustomEvents from '@web-native-js/play-ui/src/evt/CustomEvents.js';
```

To create an alias of an existing event:

```javascript
// Simply map the custom event name to an existing event name
CustomEvents.hit = 'tap';

// Now we can bind to the "hit" event instead of "tap"
on(document.body, 'hit', event => {
    console.log(event.type);
})
```

To create a full custom event implementation, provide a class with two methods: _setup_ and _teardown_. The _setup_ method will be called at the first call to bind to the event for the given element. This function will receive certain useful parameters - the emitter being the most important. The _teardown_ method will be called at the last call to unbinding the event for the given element.

```javascript
CustomEvents.tick = class {
    // When the fisrt listenr of this event
    // is attached to the element, we start ticking
    setup(el, type, emitter, hammertime) {
        this.el = el;
        this.type = type;
        this.hammertime = hammertime;
        this.intervalID = setInterval(() => {
            emitter.call();
        }, 100);
    }

    // When the last listener of this
    // event is removed, no need to keep ticking
    teardown() {
        clearInterval(this.intervalID);
    }
};

// Now we can bind to the "tick" event
on(document.body, 'tick', event => {
    console.log(event.type);
});
```

