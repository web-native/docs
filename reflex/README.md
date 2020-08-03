# Reflex

> A general-purpose reflection API for observing objects and arrays in JavaScript.

> This project now has a successor: [Observer](/observer/).

The Reflex API is a simple set of functions for detecting and responding to changes made to any JavaScript object or array. It draws its inspiration from biological [reflex actions](https://en.wikipedia.org/wiki/Reflex) and JavaScript’s set of reflection APIs like `Object.observe()` and `Reflect.set()`. To put it best, Reflex is a reflection API that is supercharged with reactivity in the form of reflex actions.

This project is opensource and [hosted on Github](https://github.com/web-native/reflex).

Take the [Getting Started](guide.md) guide or continue here for an introduction to Reflex

## Background

“Reactivity” is the modern way we describe the ability to respond to changes made to an object from other parts of an application. It has become the core of Model-Driven Views \(MDVs\) – think UI frameworks like Angular, React and Vue that data-bind objects to UI elements. In all, though, the quest for reactivity is not new; even so, the various approaches over time haven’t always been satisfactory.

Back in 2012, Adam Klein, Rafael Weinstein, and Erik Arvidsson saw light with the [`Object.observe` \(O.o\) proposal](https://arv.github.io/ecmascript-object-observe) and we had a platform-specific way to observe objects. Just three years later, however, this met with [sudden death](https://esdiscuss.org/topic/an-update-on-object-observe)! Polyfills came along to rescue the disaster and restore hope, but even the best of these wouldn’t compare. And everyone returned to some sort of means to this end.

We’ve always had native object property [setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set) and [getters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get), but due to their required setup step, the extent possible with this mechanism remains limited. [ES6 Proxies](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) and other object-wrapping methodologies \(observables here and there, and elsewhere\) do the job but present additional interoperability challenge as they obscure object identity.

On our journey building the [CHTML library](https://github.com/web-native/chtml), the quest for a flexible reactivity system rose to the top of our priorities. In an initial effort, we arrived at using [Observables](https://github.com/web-native/observable) and required that any dynamic object going into a CHTML component instance be wrapped in an _Observable_. The inflexibility quickly became obvious. To be sure, we wanted a generic event-driven mechanism that makes do with just any object. Reflex became the answer!

## The Approach

We wanted a static function, just like `Object.observe()`, that would take any object and run its observation magic on it. This way, objects do not have to be some _Observable_ with some `observe()` method. Next, without looking far, we discovered that the regular object reflection methods, like `Reflect.set()`, would be a perfect way to effect a reactive change on an object. This way, we get reactivity as an added benefit from using the familiar reflection methods. This would sum up to a library that aligns perfectly with the platform’s way of doing things.

## Goals

* Do without object wrappers or proxies, thus providing for memory efficiency and object identity.
* Follow the generic reflection pattern that decouples the object being observed from the mechanism used to observe it.
* Fire change notifications synchronously - as they happen, as an improvement to the asynchronous way of the then `Object.observe` API.

This approach positions Reflex as a general-purpose library that finds application not just in the UI, but all the way from debugging to more generic event-driven architectures in JavaScript.

[Here's how it works](guide.md)

