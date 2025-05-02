<template>
  <div class="app">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTodo" class="form">
      <input v-model="newTodo" type="text" placeholder="Tambah kegiatan..." />
      <button type="submit">Tambah</button>
    </form>

    <select v-model="filter" class="filter">
      <option value="all">Semua</option>
      <option value="pending">Belum Selesai</option>
      <option value="completed">Selesai</option>
    </select>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="index"
        :class="{ done: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  { text: 'Sarapan', completed: false },
  { text: 'Belajar Vue.js', completed: false },
  { text: 'Tidur siang', completed: true }
])

const addTodo = () => {
  const text = newTodo.value.trim()
  if (!text) return
  todos.value.push({ text, completed: false })
  newTodo.value = ''
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'pending') {
    return todos.value.filter(todo => !todo.completed)
  } else if (filter.value === 'completed') {
    return todos.value.filter(todo => todo.completed)
  } else {
    return todos.value
  }
})
</script>

<style scoped>
.app {
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;
  background: #fff;
  border-radius: 10px;
  font-family: sans-serif;
  border: 1px solid #eee;
}

h1 {
  text-align: center;
  color: #d63384;
  margin-bottom: 16px;
}

.form {
  display: flex;
  gap: 8px;
  margin-bottom: 12px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

button {
  padding: 8px 12px;
  background-color: #d63384;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background-color: #c2255c;
}

.filter {
  width: 100%;
  padding: 8px;
  margin-bottom: 12px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 8px;
  border-bottom: 1px solid #eee;
}

li.done span {
  text-decoration: line-through;
  color: #999;
}

li span {
  flex: 1;
  margin-left: 8px;
}

li button {
  background: none;
  border: none;
  color: #d63384;
  font-size: 16px;
  cursor: pointer;
}
</style>
