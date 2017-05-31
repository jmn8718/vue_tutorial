# introduction
- Vuejs is a framework to build UI
- Use virtual DOM (like react)
- Data binding: One-way

## first example
```
<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
We create a new Vue instance, it receives an *options* object which can contain options framework
- data
- template
- element (to mount the instance).
- lifecycle methods
...

## lifecycle hooks
- created and beforeCreate
- mounted and beforeMount
- updated and beforeUpdate
- destroyed and beforeDestroy

## Templates
HTML-based template syntax
Vue compiles templates into virtual DOM render functions
Can use render or template
- *String Interpolation*:
`<span>Message: {{ msg }}</span>`
- *v-once* to not update value
`<span v-once>This will never change: {{ msg }}</span>`
- Attributes
`<div v-bind:id="dynamicId"></div>`
- JS expressions (Only single expressions)
```
{{ ok ? 'YES' : 'NO' }}  Good
{{ if (ok) { return message } }}  Bad
```

# directives
- Special attributes with *v-* prefix
- Arguments denoted by a colon `v-bind:href="url" `
- Modifiers denoted by a dot `v-on:submit.prevent="onSubmit"`

- list: *v-for*, requires *key*
  - With an array
  ```
    <ul>
      <template v-for="item in items">
        <li>{{ item.msg }}</li>
        <li class="divider"></li>
      </template>
    </ul>
  ```
  - With an object
  ```
    <ul id="repeat-object" class="demo">
      <li v-for="value in object">
        {{ value }}
      </li>
    </ul>
  ```
  - Range
  ```
  <div>
    <span v-for="n in 10">{{ n }} </span>
  </div>
  ```
- if: *v-if*, *v-else*, *v-else-if*
  ```
    <template v-if="type === 'A">
      <h1>Title</h1>
      <p>Paragraph 1</p>
      <p>Paragraph 2</p>
    </template>
    <div v-else-if="type === 'B'">
      B
    </div>
    <div v-else>
      Not A/B/C
    </div>
  ```
- show/hide: *v-show*
  `<p v-show="show">show</p>`

  v-if has high toggle cost
- v-for with v-if
  ```
  <li v-for="todo in todos" v-if="!todo.isComplete">
    {{ todo }}
  </li>
  ```
- bind properties: *v-bind:*(attr) or *:*(attr)
  `<input :value="name"></input>`
- event listener: *v-on:*(event) or *@*(event)
  `<button @click="onClick">Click</div>`
- conditional classes *:class*="(object)"
  `:class="{ 'is-active': active }"`
  `:class="['highlight', isActive ? activeClass : '', errorClass]"`
- conditional styles
  `<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>`

# filters
- Can be used to apply common text formatting
- Cam be applied in:
  - mustache interpolations `{{ message | capitalize }}`
  - v-bind expressions `<div v-bind:id="rawId | formatId"></div>`
- Filters can be chained

# Event handling
- use *v-on* or shortband *@*
`<button v-on:click="increment">Add 1</button>`
- inline handling
`<button v-on:click="increment(2, $event)">Add 2</button>`
- event modifiers
  Better if not dealing with DOM event
  - .stop
  - .prevent
  - ...
  `<a v-on:click.stop.prevent="doThat"></a>`
- key modifiers
  listening for keyboard event:
  - enter
  - tab
  - esc
  - ...
  `<input @keyup.enter="submit">`





# Components
Custom elements with own templates and logic.

## Directives VS Components
Directives are meant to encapsulate DOM manipulation
Components are self-contained units that have their own view and data logic.

## Global registration
```
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})

<my-component class="baz boo"></my-component>
```
## Local registration
```
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> will only be available in parent's template
    'my-component': Child
  }
})
```
- data must be a function

## Props
- Every component has its own isolated scope
- Data can be passed down to child component
```
<child message="hello!"></child>

  props: ['message'],
  props: {
    message: String,
  }
  props: {
    message: {
      type: String,
      default: 'Message'
    }
  }
`
- dynamic props
`<input v-model="parentMsg">`
- To mutate in child component use data or computed
```
  props: ['initialCounter', 'size'],
  data: function () {
    return { counter: this.initialCounter }
  },
  computed: {
    normalizedSize: function () {
      return this.size.trim().toLowerCase()
    }
  }
```

## computed properties
computation of data
cached if doesn't change the values

## computed vs methods
We can achieve the same result
Computed will be cached, not forcing recalculate when re-render

## custom event
- Comunicate child with parent
  Trigger an event using ``$emit(eventName, values)``
- Non parent-child comunnication
  use a Vue instance
  ```
  const bus = new Vue()

  // in component A's method
  bus.$emit('id-selected', 1)

  // in component B's created hook
  bus.$on('id-selected', function (id) {
    // ...
  })
  ```

# slot
Used to distribute content in component
Content provided by parent between component tags
```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

<app-layout>
  <h1 slot="header">Here might be a page title</h1>
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>
  <p slot="footer">Here's some contact info</p>
</app-layout>

<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

# Dynamic components
Use same mount point to dinamically switch components.
Components are destoyed when unmounted
```
var vm = new Vue({
  el: '#example',
  data: {
    currentView: 'home'
  },
  components: {
    home,
    posts,
    archive
  }
})

<component v-bind:is="currentView">
  <!-- component changes when vm.currentView changes! -->
</component>

```
Use `<keep-alive>` to keep components
```
<keep-alive>
  <component :is="currentView">
    <!-- inactive components will be cached! -->
  </component>
</keep-alive>
```

# Form inputs
Use *v-model* to create two-way binding
```
<input v-model="message" placeholder="edit me">

<input
  :value="message"
  @change="message = event.target.value"
  placeholder="edit me">
```
- modifiers
  - lazy
  - number
  - trim
