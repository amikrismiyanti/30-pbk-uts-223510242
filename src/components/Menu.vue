<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['menuClick']);

const handleMenuClick = (menu) => {
  emit('menuClick', menu);
};
</script>

<template>
  <div class="menu-container">
    <button @click="handleMenuClick('Todos')" class="menu-button">
      <span class="material-icons">check_circle</span> Todos
    </button>
    <button @click="handleMenuClick('Post')" class="menu-button">
      <span class="material-icons">post_add</span> Post
    </button>
    <button @click="handleMenuClick('AlbumFoto')" class="menu-button">
      <span class="material-icons">albums</span> AlbumFoto
    </button>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');

.menu-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.menu-button {
  display: flex;
  align-items: center;
  background-color: #007bff;
  border: none;
  color: white;
  padding: 10px 20px;
  font-size: 18px;
  cursor: pointer;
  border-radius: 8px;
  transition: background-color 0.3s ease, transform 0.2s;
}

.menu-button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.menu-button .material-icons {
  margin-right: 8px;
}
</style>