<script setup>
import Navbar from './components/Navbar.vue'
import { ref } from "vue";

const tugas = ref("");
const todos = ref([
      {id : 1, namatugas : "membersihkan halaman", time : 17.00, done : true},
      {id : 2, namatugas : "belajar", time : 13.00, done : false},
      ]);

  const tambahdata = () => {
    let data = {
      id : todos.value.length + 1,
      namatugas : tugas.value,
      time : new Date().getHours(),
      done : false
    }
    todos.value.push(data);
    tugas.value = "";
  }

</script>

<template>
  <div class="app">
    <Navbar />


    <main class="main-content">
      <!-- Main content  -->
      
      <div>
        <input v-model="tugas">
        <button @click = "tambahdata">tambah tugas</button>
        
        <ul>
          <li v-for="item in todos">
              {{ item.namatugas }} - {{ item.time }} - {{ item.done }}
          </li>
        </ul>

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
</style>
