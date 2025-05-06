<script setup>
import Navbar from './components/Navbar.vue'
</script>

<template>
  <div class="app">
    <Navbar />
    <main class="main-content">
      <!-- Main content  -->
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
