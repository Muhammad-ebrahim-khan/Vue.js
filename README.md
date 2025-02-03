# Vue.js

package.json

{
  "name": "vue-todo-list",
  "version": "1.0.0",
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "vue": "^2.6.12",
    "vue-router": "^3.5.1",
    "vuex": "^3.6.2"
  },
  "devDependencies": {
    "@vue/cli-service": "^4.5.13",
    "@vue/cli-plugin-eslint": "^4.5.13",
    "eslint": "^6.8.0",
    "eslint-plugin-vue": "^6.2.2"
  }
}

src/main.js

import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

src/App.vue

<template>
  <div id="app">
    <h1>Todo List</h1>
    <todo-list />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: { TodoList }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

src/components/TodoList.vue

<template>
  <div>
    <h2>Todo List</h2>
    <ul>
      <todo-item v-for="todo in todos" :key="todo.id" :todo="todo" />
    </ul>
    <input v-model="newTodo" type="text" placeholder="Add new todo" />
    <button @click="addTodo">Add</button>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'
import { mapState, mapActions } from 'vuex'

export default {
  name: 'TodoList',
  components: { TodoItem },
  computed: {
    ...mapState(['todos'])
  },
  data() {
    return {
      newTodo: ''
    }
  },
  methods: {
    ...mapActions(['addTodoItem']),
    addTodo() {
      this.addTodoItem(this.newTodo)
      this.newTodo = ''
    }
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  margin-bottom: 10px;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
}

button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #3e8e41;
}
</style>

src/components/TodoItem.vue

<template>
  <li>
    <input type="checkbox" v-model="todo.completed" />
    <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
    <button @click="removeTodo">Remove</button>
  </li>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  name: 'TodoItem',
  props: {
    todo: Object
  },
  methods: {
    ...mapActions(['removeTodoItem']),
    removeTodo() {
      this.removeTodoItem(this.todo.id)
    }
  }
}
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>

src/store.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: 'Buy milk', completed: false },
      { id: 2, text: 'Walk the dog', completed: false },
      { id: 3, text: 'Do laundry', completed: false }
    ]
  },
  mutations: {
    addTodo(state, text) {
      state.todos.push({
