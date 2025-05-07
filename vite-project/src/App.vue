<script setup>
import Navbar from './components/Navbar.vue'
import { ref } from "vue";

const tugas = ref("");
const todos = ref([
  {id: 1, namatugas: "membersihkan halaman", time: "17:00", done: false},
  {id: 2, namatugas: "belajar", time: "13:00", done: false},
]);

const tambahdata = () => {
  if (tugas.value.trim() === "") return;
  
  const now = new Date();
  const hours = now.getHours().toString().padStart(2, '0');
  const minutes = now.getMinutes().toString().padStart(2, '0');
  const timeString = `${hours}:${minutes}`;

  let data = {
    id: todos.value.length + 1,
    namatugas: tugas.value,
    time: timeString,
    done: false
  }
  todos.value.push(data);
  tugas.value = "";
}

const toggleTask = (task) => {
  task.done = !task.done;
}

const deleteTask = (taskId) => {
  todos.value = todos.value.filter(task => task.id !== taskId);
}
</script>

<template>
  <div class="app">
    <Navbar />
    <main class="main-content">
      <div class="todo-container">
        <h1 class="page-title">All Tasks</h1>
        
        <div class="input-container">
          <input 
            v-model="tugas" 
            @keyup.enter="tambahdata"
            placeholder="Type to add a new task..."
            class="task-input"
          >
          <button @click="tambahdata" class="add-button">
            <span class="plus-icon">+</span>
          </button>
        </div>

        <div class="tasks-list">
          <div v-for="item in todos" :key="item.id" class="task-item">
            <div class="task-content">
              <input 
                type="checkbox" 
                :checked="item.done"
                @change="toggleTask(item)"
                class="task-checkbox"
              >
              <span :class="['task-text', { 'completed': item.done }]">
                {{ item.namatugas }}
              </span>
              <span class="task-time">{{ item.time }}</span>
            </div>
            <button @click="deleteTask(item.id)" class="delete-button">
              ×
            </button>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol";
  background-color: #ffffff;
  color: #37352f;
}

.app {
  display: flex;
  min-height: 100vh;
}

.main-content {
  flex: 1;
  margin-left: 240px;
  padding: 40px;
}

.todo-container {
  max-width: 800px;
  margin: 0 auto;
}

.page-title {
  font-size: 40px;
  font-weight: 700;
  margin-bottom: 30px;
  color: #37352f;
}

.input-container {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
}

.task-input {
  flex: 1;
  padding: 12px 16px;
  font-size: 16px;
  border: 1px solid #e6e6e6;
  border-radius: 4px;
  background-color: #f7f6f3;
  transition: all 0.2s ease;
}

.task-input:focus {
  outline: none;
  border-color: #37352f;
  background-color: #ffffff;
}

.add-button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: none;
  background-color: #37352f;
  color: white;
  font-size: 24px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.add-button:hover {
  background-color: #2a2925;
}

.tasks-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background-color: #f7f6f3;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.task-item:hover {
  background-color: #f1f1ef;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 12px;
  flex: 1;
}

.task-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #37352f;
}

.task-text {
  font-size: 16px;
  color: #37352f;
}

.task-text.completed {
  text-decoration: line-through;
  color: #9b9a97;
}

.task-time {
  font-size: 14px;
  color: #9b9a97;
  margin-left: auto;
  padding-right: 16px;
}

.delete-button {
  background: none;
  border: none;
  color: #9b9a97;
  font-size: 20px;
  cursor: pointer;
  padding: 4px 8px;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.delete-button:hover {
  color: #37352f;
  background-color: #e6e6e6;
}
</style>
