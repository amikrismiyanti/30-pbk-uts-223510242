<script setup>
import { ref } from 'vue';
import Todos from './components/Todos.vue';
import Post from './components/Post.vue';

const selectedMenu = ref('');

const handleMenuClick = (menu) => {
  selectedMenu.value = menu;
};
</script>

<template>
  <div>
    <button @click="handleMenuClick('Todos')">Todos</button>
    <button @click="handleMenuClick('Post')">Post</button>

    <div>
      <Todos v-if="selectedMenu === 'Todos'" />
      <Post v-if="selectedMenu === 'Post'" />
      <p v-if="!selectedMenu">No menu selected</p>
    </div>
  </div>
</template>

<style scoped>
  button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin-right: 10px;
    cursor: pointer;
    border-radius: 5px;
  }

  div {
    background-color: #f2f2f2;
    padding: 20px;
    border-radius: 5px;
    margin: 20px auto;
    max-width: 600px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

  div > div {
    margin-top: 20px;
  }

  p {
    color: #666;
    font-style: italic;
  }
</style>