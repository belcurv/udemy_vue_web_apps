# Vue JS 2.0 - Mastering Web Apps

Course repo: https://github.com/15Dkatz/vue-guides

Vue CDN: https://unpkg.com/vue

## Section 2 - Understanding Vue Syntax and Essentials

### The Vue Instance

Get a Vue instance by calling the Vue constructor, which takes an options object as it's argument. Within that object, we define a few things:

1.  the element we want to target
2.  data to give that target, in the form of an object

```js
var app = new Vue({
    el : '#app',
    data: {
        text : 'Hello Vue'
    }
});
```

Then we can interpolate that text in the markup using double curly braces:

```html
<div id="app">{{text}}</div>
```

### Directives

Allow us to add reactivity to pieces of the rendered DOM.

Vue directives are denoted by the `v-` syntax.

Example:

```html
<div id="app">
    <div
       v-bind:title="randNum"
       v-if="shown">
        Hover over me for your random number
    </div>
</div>

<script>
    var app = new Vue({
        el   : '#app',
        data : {
            randNum : Math.floor(Math.random() * 50) +1,
            shown   : false
        }
    });
</script>
```

**`v-bind`** - this attribute _binds_ the result of an expression (`randNum`) to the target element's `title` attribute (what appears when you hover over elements of a web page).

**`v-if`**: instrumental in conditional rendering.

**`v-for`**: looping over data and rendering to DOM. Example:

```
<div id="app">
    <ul>
        <li v-for="color in colors">{{color}}</li>
    </ul>
</div>

<script>
    var app = new Vue({
        el   : '#app',
        data : {
            colors: ['red', 'green', 'blue', 'yellow', 'orange']
        }
    });
</script>
```

`v-for` takes an optional 2nd paramter that keeps track of the index in our loop:

```html
<li v-for="(color, index) in colors">{{color}}</li>
```

**`v-on`** - Click binding.

We can give buttons functionality using a method in the `methods` property in the Vue instance options object.

```
<div id="app">
    <button class="vue-btn" v-on:click="reveal">Reveal Secret Number</button>
</div>

<script>
    var app = new Vue({
        el   : '#app',
        data : {
            secretNumber: Math.floor(Math.random() * 100) + 1
        },
        methods : {
            reveal() {
                alert(`Here's the secret number: ${this.secretNumber}`)
            }
        }
    });
</script>
```

`data` in a Vue instance is accessible using the `this` keyword. This is because the `data` object itself is bound to the Vue instance.

Shorthand syntax for `v-on` directive: `@click`

```
// equivalent:
<button v-on:click="reveal">
<button @click="reveal">
```

### Components

Custom tags and reusable components.

Start by declaring a component instance:

```js
var counter = Vue.component('counter', {
    template: `
        <div>
            <div>{{this.count}}</div>
            <button class="vue-btn" @click="increment">Increment</button>
        </div>
    `,
    data() {
        return {
            count: 0
        };
    },
    methods: {
        increment() {
            this.count++
        }
    }
});
```

A Vue component takes 2 paramters:

1.  the name of the component
2.  an options object consisting of `template`,  `data`, `methods`.

Unlike a Vue instance, `data()` in a Vue component **must be a function**. This is different from the data property in a Vue instance. This is to create a specific `this` object for the component, giving a local Vue component its own "state".

Then we need to add the component to our Vue instance, adding it to a `components` property of the instance. The `components` property is an object consisting of all the component names:

```js
// instance
var app = new Vue({
    el   : '#app',
    components: {
        counter  // es6 single-name object syntax
    }
});
```

And then adding the component in the markup:

```html
<div id="app">
    <counter></counter>
</div>
```

Components can also receive **props** - parents can give components some data. First, add a `props` field to the component's options object. Properties in the `props` field are themselves objects. Specify a `type` and `default`:

```js
// component
var counter = Vue.component('counter', {
    template: `
        <div>
            <div>Count: {{this.count}}</div>
            <button class="vue-btn" @click="increment">
                Increment by {{this.addNum}}  // changed to reflect addNum
            </button>
        </div>
    `,
    props: {
        addNum: {
            type    : Number,
            default : 1
        }
    },
    data() {
        return {
            count: 0
        };
    },
    methods: {
        increment() {
            this.count += this.addNum  // changed to increment by addNum
        }
    }
});
```

But what if we wanted to add 5? We specify the `props` property in the component markup, kebab cased:

```html
<div id="app">
    <counter add-num="5"></counter>
</div>
```

Now, the above will actually throw a warning in console. Because we're passing in a String but `addNum` expects a type: `Number`. To fix this, we add a `v-bind`:

```html
<div id="app">
    <counter v-bind:add-num="5"></counter>
</div>
```

### `v-model` and Computed Properties

Models allow the user to update state in the view component with something like an input field.

Computed properties allow us to compute some data with complex logic. Ex. it may take some info from the original data object and return a modified form of it.

**`v-model`** - appears to be like Angular's 2-way data binding:

```html
<div id="app">
    <span>Change the text</span>
    <input v-model="text" type="text" placeholder="text...">
</div>

<script>

    // instance
    var app = new Vue({
        el   : '#app',
        data: {
            text: ''
        }
    });

</script>
```

Computed properties - add a `computed` property to the instance options object. Within it are **functions**. And then you can interpolate the results of computed properties in your markup:

```html
<div id="app">
    <span>Change the text</span>
    <input v-model="text" type="text" placeholder="text...">
    <div>Lowercased: {{lowerText}}</div>
</div>

<script>

    // instance
    var app = new Vue({
        el   : '#app',
        data: {
            text: ''
        },
        computed: {
            lowerText() {
                return this.text.toLowerCase()
            }
        }
    });

</script>
```

### Lifecycle Hooks

Lifecycle hooks in Vue components and instances allow us to run specific code at specific times in an application. We can take advantage of these hooks when a component renders, or when portions of the state get updated.

One of the most common hooks is the **created** hook, which is a function in a Vue instance or component that runs when it is created.

```html
<div id="app">
    <div class="">Hello World</div>
</div>

<script>

    // instance
    var app = new Vue({
        el   : '#app',
        data : {

        },
        created() {
            alert(`The time is ${Date.now().toLocaleString()}`)
        }
    });

</script>
```

In the above, the `created()` function will fire **before*8 the inner "Hello World" DIV gets rendered.

Vue **mounted** lifecycle hook - called once a component gets attached to the DOM.

**updated** hook - fires every time a piece of our data gets updated throughout the life of a component. `updated()` depends on the DOM element/component changing. In the following, if you **don't** interpolate `{{count}}` in the app DIV, `updated` does not fire.

```html
<div id="app">
    <div class="">Count: {{count}}</div>
    <button class="vue-btn" @click="increment">Increment</button>
</div>

<script>

    // instance
    var app = new Vue({
        el   : '#app',
        data : {
            count: 0
        },
        methods: {
            increment() {
                this.count++
            }
        },
        updated() {
            console.log('this.count', this.count)
        }
    });

</script>
```

### Vue CLI

Creating a new Vue application based on the Webpack simple template:

```
$ vue init webpack-simple starbase
```