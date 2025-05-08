<script setup>
import Navbar from './components/Navbar.vue'
import { ref, computed } from "vue";

const tugas = ref("");
const todos = ref([
  {id: 1, namatugas: "membersihkan halaman", time: "17:00", done: false, date: "2025-02-14"},
  {id: 2, namatugas: "belajar", time: "13:00", done: false, date: "2025-02-13"},
]);

const currentPage = ref('all');

const tambahdata = () => {
  if (tugas.value.trim() === "") return;
  const now = new Date();
  const hours = now.getHours().toString().padStart(2, '0');
  const minutes = now.getMinutes().toString().padStart(2, '0');
  const timeString = `${hours}:${minutes}`;
  const dateString = now.toISOString().split('T')[0];
  let data = {
    id: todos.value.length + 1,
    namatugas: tugas.value,
    time: timeString,
    done: false,
    date: dateString
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

const completedTasks = computed(() => todos.value.filter(t => t.done));

const formatDate = (dateStr) => {
  if (!dateStr) return '';
  const d = new Date(dateStr);
  return d.toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' });
}
</script>

<template>
  <div class="app">
    <Navbar :currentPage="currentPage" @changePage="(p) => currentPage = p" />
    <main class="main-content">
      <div v-if="currentPage === 'all'" class="todo-container">
        <h1 class="page-title">All Tasks</h1>
        <div class="input-container">
          <div class="input-preview">
            <span v-if="tugas">{{ tugas }}</span>
            
          </div>
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
      <div v-else-if="currentPage === 'completed'" class="completed-container">
        <h1 class="page-title">Completed Tasks</h1>
        <div class="notion-table-wrapper">
          <table class="notion-table">
            <thead>
              <tr>
                <th>Task Name</th>
                <th>Date</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in completedTasks" :key="item.id">
                <td>
                  <span class="notion-pill">{{ item.namatugas }}</span>
                </td>
                <td>{{ formatDate(item.date) }}</td>
                <td>
                  <span class="notion-status">
                    <span class="notion-dot"></span>
                    Done
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
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
  flex-direction: column;
  gap: 4px;
  margin-bottom: 24px;
  align-items: flex-start;
  width: 100%;
  max-width: 400px;
}

.input-preview {
  min-height: 24px;
  font-size: 16px;
  color: #37352f;
  margin-bottom: 0;
  padding-left: 2px;
  width: 100%;
  word-break: break-word;
}

.input-preview .placeholder {
  color: #bdbdbd;
}

.task-input {
  width: 100%;
  box-sizing: border-box;
}

.add-button {
  align-self: flex-end;
  margin-top: 8px;
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

.completed-container {
  max-width: 900px;
  margin: 0 auto;
}
.notion-table-wrapper {
  background: #191919;
  border-radius: 6px;
  padding: 24px 16px 16px 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}
.notion-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  background: #191919;
  color: #fff;
  font-size: 15px;
  border-radius: 6px;
  overflow: hidden;
}
.notion-table th, .notion-table td {
  border-bottom: 1.5px solid #232323;
  padding: 10px 16px;
  text-align: left;
}
.notion-table th {
  background: #191919;
  font-weight: 600;
  color: #bdbdbd;
  border-bottom: 2px solid #232323;
}
.notion-table tr:last-child td {
  border-bottom: none;
}
.notion-pill {
  background: #2f2f2f;
  color: #ffb86c;
  border-radius: 4px;
  padding: 2px 10px;
  font-weight: 500;
  font-size: 14px;
  display: inline-block;
}
.notion-status {
  display: inline-flex;
  align-items: center;
  background: #1a3d2a;
  color: #6ee7b7;
  border-radius: 4px;
  padding: 2px 12px 2px 8px;
  font-weight: 600;
  font-size: 14px;
}
.notion-dot {
  width: 9px;
  height: 9px;
  background: #21c97a;
  border-radius: 50%;
  display: inline-block;
  margin-right: 7px;
}

.completed-container .page-title {
  color: #fff;
}
</style>
