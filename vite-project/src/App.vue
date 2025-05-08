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
const pendingTasks = computed(() => todos.value.filter(t => !t.done));

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
      <div v-else-if="currentPage === 'pending'" class="completed-container">
        <h1 class="page-title">Pending Tasks</h1>
        <div class="notion-table-wrapper">
          <table class="notion-table">
            <thead>
              <tr>
                <th></th>
                <th>Task Name</th>
                <th>Date</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="pendingTasks.length === 0">
                <td colspan="4">
                  <div class="empty-state">
                    <div class="empty-state-icon">📝</div>
                    <p>No pending tasks</p>
                  </div>
                </td>
              </tr>
              <tr v-for="item in pendingTasks" :key="item.id">
                <td>
                  <input 
                    type="checkbox" 
                    :checked="item.done"
                    @change="toggleTask(item)"
                    class="task-checkbox"
                  >
                </td>
                <td>
                  <span class="notion-pill">{{ item.namatugas }}</span>
                </td>
                <td>{{ formatDate(item.date) }}</td>
                <td>
                  <span class="notion-status undone">
                    <span class="notion-dot red"></span>
                    Undone
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
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
              <tr v-if="completedTasks.length === 0">
                <td colspan="3">
                  <div class="empty-state">
                    <div class="empty-state-icon">✓</div>
                    <p>No completed tasks</p>
                  </div>
                </td>
              </tr>
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
  background: #f7f6f3;
  min-height: 100vh;
}

.todo-container, .completed-container {
  max-width: 900px;
  margin: 0 auto;
  background: #191919;
  border-radius: 6px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.page-title {
  font-size: 40px;
  font-weight: 700;
  margin-bottom: 30px;
  color: #fff;
  letter-spacing: -0.5px;
}

.input-container {
  display: flex;
  flex-direction: column;
  gap: 4px;
  margin-bottom: 24px;
  align-items: flex-start;
  width: 100%;
  max-width: 400px;
  background: #2f2f2f;
  padding: 16px;
  border-radius: 6px;
  transition: all 0.3s ease;
}

.input-container:focus-within {
  background: #363636;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.input-preview {
  min-height: 24px;
  font-size: 16px;
  color: #fff;
  margin-bottom: 0;
  padding-left: 2px;
  width: 100%;
  word-break: break-word;
  opacity: 0.8;
}

.input-preview .placeholder {
  color: #bdbdbd;
}

.task-input {
  width: 100%;
  box-sizing: border-box;
  background: transparent;
  border: none;
  color: #fff;
  font-size: 16px;
  padding: 8px 0;
  outline: none;
}

.task-input::placeholder {
  color: #bdbdbd;
  opacity: 0.7;
}

.add-button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: none;
  background: #37352f;
  color: white;
  font-size: 24px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  margin-top: 8px;
}

.add-button:hover {
  background: #2a2925;
  transform: translateY(-1px);
}

.tasks-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-top: 24px;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: #2f2f2f;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.task-item:hover {
  background: #363636;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 12px;
  flex: 1;
}

.task-checkbox {
  width: 16px;
  height: 16px;
  cursor: pointer;
  accent-color: #37352f;
  margin: 0;
  border-radius: 3px;
  border: 1px solid #37352f;
  transition: all 0.2s ease;
}

.task-checkbox:checked {
  background-color: #37352f;
  border-color: #37352f;
  animation: checkmark 0.2s ease-in-out;
}

@keyframes checkmark {
  0% {
    transform: scale(0.8);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

.task-text {
  font-size: 16px;
  color: #fff;
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
  color: #fff;
  background: rgba(255, 255, 255, 0.1);
}

.notion-table-wrapper {
  background: #191919;
  border-radius: 6px;
  padding: 24px 16px 16px 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.notion-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  background: transparent;
  color: #fff;
  font-size: 15px;
  border-radius: 6px;
  overflow: hidden;
  border: 1px solid #232323;
}

.notion-table th {
  background: #232323;
  font-weight: 600;
  color: #bdbdbd;
  border-bottom: 2px solid #232323;
  padding: 14px 16px;
  text-transform: uppercase;
  font-size: 12px;
  letter-spacing: 0.5px;
}

.notion-table td {
  padding: 14px 16px;
  border-bottom: 1px solid #232323;
}

.notion-table tr:hover {
  background-color: #232323;
}

.notion-pill {
  background: #2f2f2f;
  color: #ffb86c;
  border-radius: 4px;
  padding: 4px 12px;
  font-size: 13px;
  letter-spacing: 0.3px;
  font-weight: 500;
  display: inline-block;
}

.notion-pill:hover {
  background: #363636;
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0,0,0,0.15);
}

.notion-status {
  display: inline-flex;
  align-items: center;
  background: #1a3d2a;
  color: #6ee7b7;
  border-radius: 4px;
  padding: 4px 12px 4px 8px;
  font-weight: 600;
  font-size: 14px;
}

.notion-status:hover {
  filter: brightness(1.1);
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0,0,0,0.15);
}

.notion-status.undone {
  background: #3d1a1a;
  color: #ff6b6b;
}

.notion-dot {
  width: 9px;
  height: 9px;
  background: #21c97a;
  border-radius: 50%;
  display: inline-block;
  margin-right: 7px;
}

.notion-dot.red {
  background: #ff4d4d;
}

.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: #bdbdbd;
  font-size: 16px;
  background: #232323;
  border-radius: 4px;
  margin: 8px;
}

.empty-state-icon {
  font-size: 32px;
  margin-bottom: 16px;
  opacity: 0.5;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-5px);
  }
  100% {
    transform: translateY(0px);
  }
}
</style>
