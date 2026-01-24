# 第13回 Webエンジニアリング演習 レポート
## 4723217
## 実装した内容
main.ts
'''
//import './assets/main.css'

import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
import router from './router'

const app = createApp(App)
const pinia = createPinia()
app.use(pinia)
app.use(router)
app.mount('#app')

'''

index.ts
'''
import { createRouter, createWebHistory } from 'vue-router'
import TodoListView from '../views/TodoListView.vue'
import About from '../views/About.vue'
import TodoDetail from '../views/TodoDetail.vue'


const routes = [
  { path: '/', name: 'TodoList', component: TodoListView },
  { path: '/about', name: 'About', component: About },
  { path: '/todos/:id', name: 'TodoDetail', component: TodoDetail }
]
export const router = createRouter({
  history: createWebHistory(),
  routes
})
 


export default router
'''

App.vue
'''
<script setup lang=ts>
  import { ref, computed } from 'vue';
  import AddTodo from './components/AddTodo.vue';
  import { useTodosStore } from './stores/todoStore';
  import { RouterView, RouterLink } from 'vue-router';

  const name = ref('Vue 3 with TypeScript');

  const todoStore = useTodosStore();

</script>

<template>
  <div id="app">
    <header>
      <h1>Todo App</h1>
    </header>

    <main id ="main-content">
      <router-view />
    </main>

    <footer>
      <ul>
        <li><router-link to="/">Todo List</router-link></li>
        <li><router-link to="/about">About</router-link></li>
      </ul>  
    </footer>
  </div>
</template>

<style scoped> 
  #main-content { padding: 1rem; border-style: solid; border-width: 2px; }
</style> 
'''

TodoListView.vue
'''
<script setup lang="ts">
    import { useTodosStore } from '../stores/todoStore';
    import AddTodo from '../components/AddTodo.vue'
    import { RouterLink } from 'vue-router'
    const todoStore = useTodosStore();
</script>

<template>
    <section>
        <h2>Todos</h2>
        <ul>
        <li
            v-for="todo in todoStore.todos"
            :key="todo.id"
            style="display:flex; align-items:center; gap:0.5rem; margin:0.25rem 0;"
        >
            <input type="checkbox" v-model="todo.completed" />
            <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">
                <RouterLink :to="{ name: 'TodoDetail', params: { id: todo.id } }">{{ todo.title }}</RouterLink>
            </span>
            <button v-on:click="todoStore.removeTodo(todo.id)">Remove</button>
        </li>
        </ul>
    </section>

    <section>
        <AddTodo />
    </section>
</template>
'''

About.vue
'''
<template>
    <div class="about">
        <h1>About This App</h1>
        <p>This is a simple Todo application built with Vue 3, using Composition API, Pinia for state management, and Vue Router for navigation.</p>
        <p>You can add, view, and manage your todos.</p>
    </div>
</template>

<script setup lang="ts">
// No script needed for this simple view
</script>

<style scoped>
.about {
    padding: 20px;
    text-align: center;
}
</style>
'''

TodoDetail.vue
'''
<script setup lang="ts">
import { computed } from 'vue';
import { useRoute } from 'vue-router';
import { useTodosStore } from '../stores/todoStore';

const route = useRoute();
const todoStore = useTodosStore();

const todoId = computed(() => parseInt(route.params.id as string));
const todo = computed(() => todoStore.todos.find(t => t.id === todoId.value));
</script>

<template>
    <div v-if="todo">
        <h2>Todo Detail</h2>
        <p><strong>Title:</strong> {{ todo.title }}</p>
        <p><strong>Description:</strong> {{ todo.description }}</p>
        <p><strong>Completed:</strong> {{ todo.completed ? 'Yes' : 'No' }}</p>
    </div>
    <div v-else>
        <p>Todo not found.</p>
    </div>
</template>
'''

todoStore.ts
'''
import { defineStore } from 'pinia'
import { ref } from 'vue'

interface Todo {
    id: number
    title: string
    description: string
    completed: boolean
}


export const useTodosStore = defineStore('todos', ()=>{
    let serialId = 4

    const todos = ref<Todo[]>([
        { id: 1, title: 'Buy groceries', description: 'Get milk, bread, and eggs', completed:false},
        { id: 2, title: 'Write report', description: 'Finish the quarterly report', completed:true},
        { id: 3, title: 'Call Alice', description: 'Discuss the project', completed:false},
    ])

    const addTodo = (title: string, description: string) => {
        const newTodo: Todo = {
            id: serialId++,
            title: title,
            description: description,
            completed: false,
        }
        todos.value.push(newTodo)
    }

    const removeTodo = (id: number) => {
        todos.value = todos.value.filter(todo => todo.id !== id)
    }

    return { todos, addTodo, removeTodo }
})
'''