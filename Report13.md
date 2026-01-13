# 第13回 Webエンジニアリング演習 レポート
## 学籍番号
（4723217）
## 実装したコード
main.ts
```
//import './assets/main.css'

import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)
const pinia = createPinia()

app.use(pinia)
app.mount('#app')
```

App.vue
```
<script setup lang=ts>
import { ref, computed } from 'vue';
import AddTodo from './components/AddTodo.vue';
import { useTodosStore } from './stores/todoStore';

const name = ref('Vue 3 with TypeScript');

const todoStore = useTodosStore();
</script>

<template>
  <div id="app">
    <section class="todo-app">
      <h2>Todos</h2>
      <ul>
        <li
          v-for="todo in todoStore.todos"
          :key="todo.id"
          style="display:flex; align-items:center; gap:0.5rem; margin:0.25rem 0;"
        >
          <input type="checkbox" v-model="todo.completed" />
          <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">
            {{ todo.title }}
          </span>
          <button v-on:click="todoStore.removeTodo(todo.id)">Delete</button>
        </li>
      </ul>
    </section>
    <section>
      <AddTodo />
    </section>
  </div>
</template>
```

todoStore.ts
```
import { defineStore } from 'pinia'
import { ref } from 'vue'

interface Todo {
    id: number
    title: string
    completed: boolean
}


export const useTodosStore = defineStore('todos', ()=>{
    let serialId = 4

    const todos = ref<Todo[]>([
        { id: 1, title: 'Buy groceries', completed:false},
        { id: 2, title: 'Write report', completed:true},
        { id: 3, title: 'Call Alice', completed:false},
    ])

    const addTodo = (title: string) => {
        const newTodo: Todo = {
            id: serialId++,
            title: title,
            completed: false,
        }
        todos.value.push(newTodo)
    }

    const removeTodo = (id: number) => {
        todos.value = todos.value.filter(todo => todo.id !== id)
    }

    return { todos, addTodo, removeTodo }
})
```

AddTodo.vue
```
<script setup lang="ts">
import { ref } from 'vue';
import { useTodosStore } from '../stores/todoStore';

const todoStore = useTodosStore();

const newTitle = ref<string>('');

</script>

<template>
    <section>
      <h2>New Todo</h2>
      <div style="display: flex; align-items: center; gap: 0.5rem; margin: 0.25rem 0;">
        <input type="text" v-model="newTitle" placeholder="New Todo title" aria-label="New Todo title" />
      <button v-on:click="todoStore.addTodo(newTitle)" :disabled="!newTitle">Add</button>
      </div>
    </section>
</template>
```