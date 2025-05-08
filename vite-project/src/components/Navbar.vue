<script setup>
const props = defineProps({
  currentPage: String
})
const emit = defineEmits(['changePage'])
</script>

<template>
  <nav class="navbar">
    <div class="navbar-content">
      <div class="logo">
        <h1>PRODO</h1>
      </div>
      <div class="nav-items">
        <div 
          class="nav-item" 
          :class="{ active: currentPage === 'all' }"
          @click="$emit('changePage', 'all')"
        >
          <span class="nav-icon">📝</span>
          <span class="nav-text">All Tasks</span>
        </div>
        <div 
          class="nav-item" 
          :class="{ active: currentPage === 'completed' }"
          @click="$emit('changePage', 'completed')"
        >
          <span class="nav-icon">✓</span>
          <span class="nav-text">Completed</span>
        </div>
        <div class="nav-item">
          <span class="nav-icon">⏳</span>
          <span class="nav-text">Pending</span>
        </div>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  width: 240px;
  height: 100vh;
  background-color: #f7f6f3;
  border-right: 1px solid #e6e6e6;
  position: fixed;
  left: 0;
  top: 0;
  padding: 20px 0;
}

.navbar-content {
  padding: 0 16px;
}

.logo {
  padding: 10px 0 20px 0;
  margin-bottom: 20px;
  border-bottom: 1px solid #e6e6e6;
}

.logo h1 {
  font-size: 24px;
  font-weight: 600;
  color: #37352f;
  margin: 0;
  letter-spacing: -0.5px;
}

.nav-items {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.nav-item {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
  color: #37352f;
}

.nav-item:hover {
  background-color: #e9e9e7;
}

.nav-item.active {
  background-color: #e9e9e7;
}

.nav-icon {
  margin-right: 8px;
  font-size: 16px;
}

.nav-text {
  font-size: 14px;
  font-weight: 500;
}
</style> 