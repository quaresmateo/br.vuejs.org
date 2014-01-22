title: Listening for Events
type: guide
order: 4
---

# {{title}}

You can bind `v-on` to either a handler function (without the invocation parentheses) or an expression:

``` html
<div id="demo">
    <a v-on="click: onClick">Trigger a handler</a>
    <a v-on="click: n++">Trigger an expression</a>
</div>
```

``` js
new Vue({
    el: '#demo',
    data: {
        n: 0
    },
    methods: {
        onClick: function (e) {
            console.log(e.targetVM === this) // true
        }
    }
})
```

As shown in the example above, if a handler function is provided, it will get the original DOM event as the argument. The event also comes with an extra property: `targetVM`, pointing to the particular ViewModel the event was triggered on. This could be useful when `v-on` is used with `v-repeat`, since the latter creates a lot of child ViewModels. However, it is often more convenient to use an invocation expression passing in `this`, which equals the current context ViewModel:

``` html
<ul id="list">
    <li v-repeat="items" v-on="click: toggle(this)">{{text}}</li>
</ul>
```

``` js
new Vue({
    el: '#list',
    data: {
        items: [
            { text: 'one', done: true },
            { text: 'two', done: false }
        ]
    },
    methods: {
        toggle: function (item) {
            item.done = !item.done
        }
    }
})
```

You might be concerned about this whole event listening approach violates the good old rules about "separation of concern". Rest assured - since all Vue.js handler functions and expressions are strictly bound to the ViewModel that's handling the current View, it won't cause any maintainance difficulty. In fact, there are several benefits in using `v-on`:

1. It makes it easier to locate the handler function implementations within your JS code by simply skimming the HTML template.
2. Since you don't have to manually attach event listeners in JS, your ViewModel code can be pure logic and DOM-free. This makes it easier to test.
3. When a ViewModel is destroyed, all event listeners are automatically removed.
4. When `v-on` is used in conjunction with `v-repeat`, Vue.js will automatically use event delegation instead of attaching a listner on every repeated instance.

Next up: [Handling Forms](/guide/forms.html).