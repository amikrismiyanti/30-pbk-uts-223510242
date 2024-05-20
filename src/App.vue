<!-- App.vue -->
<script setup>
import { ref } from 'vue';
import Menu from './components/Menu.vue';
import Todos from './components/Todos.vue';
import Post from './components/Post.vue';

const selectedMenu = ref('');

const handleMenuClick = (menu) => {
  selectedMenu.value = menu;
};
</script>

<template>
  <div class="app-container">
    <Menu @menuClick="handleMenuClick" />
    <div class="content-container">
      <Todos v-if="selectedMenu === 'Todos'" />
      <Post v-if="selectedMenu === 'Post'" />
      <p v-if="!selectedMenu" class="no-menu-selected">No menu selected</p>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(135deg, #f8f9fa, #e0e0e0);
  min-height: 100vh;
  font-family: 'Roboto', sans-serif;
  padding: 20px;
}

.content-container {
  width: 100%;
  max-width: 800px;
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
}

.no-menu-selected {
  color: #666;
  font-style: italic;
  text-align: center;
}
</style>
