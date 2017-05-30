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

# Directives VS Components
Directives are meant to encapsulate DOM manipulation
Components are self-contained units that have their own view and data logic.

# directives
- list: *v-for*
  `<li v-for="note in notes">{{ note.text }}</li>`
- if: *v-if* and *v-else*
  `<input :value="name"></input>`
- show/hide: *v-show*
  `<p v-show="show">show</p>`

- bind properties: *v-bind:*(event) or *:*(attr)
  `<input :value="name"></input>`
- event listener: *v-on:*(event) or *@*(event)
  `<button @click="onClick">Click</div>`
- conditional classes *:class*="(object)"
  `:class="{ 'is-active': active }"`
# list

# lifecycle methods

# computed properties
computation of data
filtering data
cached if doesn't change the values
