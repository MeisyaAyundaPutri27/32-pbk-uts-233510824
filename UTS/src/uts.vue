<template>
  <div class="outer-wrapper">
    <div class="container">
      <h1>📋 Daftar Kegiatan</h1>

      <form @submit.prevent="addTodo" class="form">
        <div class="input-wrapper">
          <input v-model="newTodo" type="text" placeholder="Masukkan kegiatan..." />
        </div>
        <div class="button-wrapper">
          <button type="submit">Tambah</button>
        </div>
      </form>

      <div class="dropdown-filter">
        <select v-model="filter">
          <option value="all">📄 Semua Kegiatan</option>
          <option value="pending">⏳ Belum Selesai</option>
          <option value="completed">✅ Sudah Selesai</option>
        </select>
      </div>

      <ul>
        <li
          v-for="(todo, index) in filteredTodos"
          :key="'todo-' + index"
          :class="{ done: todo.completed }"
        >
          <input type="checkbox" v-model="todo.completed" />
          <span>{{ todo.text }}</span>
          <button class="delete" @click="removeTodo(index)">
            🗑️ <!-- Ikon Tempat Sampah -->
          </button>
        </li>
      </ul>
    </div>

    <footer class="footer">
      <marquee behavior="scroll" direction="left">© 2025 MEISYA AYUNDA PUTRI</marquee>
    </footer>
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
  if (text === '') return
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
.outer-wrapper {
  background: linear-gradient(to bottom right, #ffe0f0, #f3e8ff);
  padding: 40px 20px;
  min-height: 100vh;
}

.container {
  max-width: 960px;
  margin: 0 auto;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 20px;
  padding: 24px;
  backdrop-filter: blur(15px);
  -webkit-backdrop-filter: blur(15px);
  box-shadow: 0 12px 30px rgba(214, 51, 132, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.4);
  font-family: 'Segoe UI', sans-serif;
}

h1 {
  text-align: center;
  color: #d63384;
  margin-bottom: 24px;
  font-size: 48px;
  font-weight: bold;
  text-shadow: 1px 1px 2px rgba(214, 51, 132, 0.2);
}

.form {
  display: flex;
  gap: 16px;
  margin-bottom: 16px;
}

.input-wrapper {
  flex: 1;
}

input[type="text"] {
  width: 100%;
  padding: 12px 20px;
  font-size: 16px;
  border: 2px solid #f783ac;
  border-radius: 12px;
  background: rgba(255, 245, 249, 0.8);
  outline: none;
  height: 48px;
  box-sizing: border-box;
  box-shadow: inset 1px 1px 3px rgba(0,0,0,0.05);
}

input[type="text"]:focus {
  border-color: #da77f2;
  background-color: rgba(252, 239, 249, 0.9);
}

.button-wrapper {
  flex-shrink: 0;
}

button {
  background: linear-gradient(to right, #f783ac, #da77f2);
  color: white;
  font-weight: bold;
  border: none;
  padding: 0 24px;
  height: 48px;
  font-size: 16px;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 10px rgba(218, 119, 242, 0.4);
}

button:hover {
  transform: scale(1.05);
}

.dropdown-filter {
  margin-bottom: 20px;
  display: flex;
  justify-content: flex-start;
}

select {
  padding: 10px 14px;
  font-size: 16px;
  border-radius: 12px;
  border: 2px solid #f783ac;
  background-color: rgba(255, 240, 246, 0.8);
  color: #5f3dc4;
  backdrop-filter: blur(6px);
  box-shadow: 0 2px 6px rgba(247, 131, 172, 0.2);
}

ul {
  list-style: none;
  padding: 0;
  margin-top: 10px;
}

li {
  display: flex;
  align-items: center;
  padding: 12px;
  background: rgba(255, 255, 255, 0.8);
  margin-bottom: 10px;
  border-left: 5px solid #da77f2;
  border-radius: 12px;
  transition: all 0.3s ease;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.05);
}

li.done {
  background-color: #e9ecef;
  border-left-color: #adb5bd;
}

li.done span {
  text-decoration: line-through;
  color: #868e96;
}

li span {
  flex: 1;
  margin-left: 10px;
  font-size: 16px;
}

.delete {
  background: transparent;
  border: none;
  cursor: pointer;
  transition: 0.2s ease;
}

.delete i {
  font-size: 18px;
  color: #d63384;
}

.delete:hover i {
  color: #c92a2a;
}

.footer {
  margin-top: 30px;
  text-align: center;
  font-weight: bold;
  font-size: 14px;
  color: #d63384;
  padding: 10px;
}
</style>