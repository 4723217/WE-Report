# 第12回 Webエンジニアリング演習レポート
## 学籍番号（名前は 書かなくてよい）
（4723217）

## 本日の 感想
関数関連のとこがやっぱり苦手だなと感じました。

## 実装したコード
AddTodo.vue
```
<script setup lang="ts">
import { ref } from 'vue'


type Todo = { id: number; title: string; completed: boolean };
const todos = defineModel<Todo[]>("todos")

const newTitle = ref('');
const nextId = ref(Math.max(0, ...todos.value.map((t) => t.id)) + 1);

function addTodo() {
  const title = newTitle.value;
  todos.value.push({ id: nextId.value++, title, completed: false });
  newTitle.value = '';
}
</script>

<template>
    <section>
      <h2>New Todo</h2>
      <div style="display: flex; align-items: center; gap: 0.5rem; margin: 0.25rem 0;">
        <input type="text" v-model="newTitle" placeholder="New todo title" aria-label="New todo title" />
      <button v-on:click="addTodo" :disabled="!newTitle">Add</button>
      </div>
    </section>
</template>
```

App.vue
```
<script setup lang=ts>
import { ref, computed } from 'vue';

const name = ref('Vue 3 with TypeScript');

type Todo = { id: number; title: string; completed: boolean };

const todos = ref<Todo[]>([
  { id: 1, title: 'Buy groceries', completed: false },
  { id: 2, title: 'Write report', completed: true },
  { id: 3, title: 'Call Alice', completed: false },
]);

const count = computed(
  function(): number {
    return todos.value.filter((i) => i.completed == false).length
  }
)

import AddTodo from './components/AddTodo.vue'

</script>

<template>
  <div id="app">

    <section class="todo-app">
      <h2>Todos {{count}}</h2>

      <ul>
        <li
          v-for="todo in todos"
          :key="todo.id"
          style="display:flex; align-items:center; gap:0.5rem; margin:0.25rem 0;"
        >
          <input type="checkbox" v-model="todo.completed" />
          <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">
            {{ todo.title }}
          </span>
        </li>
      </ul>
    </section>

    <AddTodo v-model:todos="todos"/>
    <AddTodo v-model:todos="todos"/>
    <AddTodo v-model:todos="todos"/>


  </div>
</template>
```

