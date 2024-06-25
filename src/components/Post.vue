<script setup>
import { ref, onMounted } from 'vue';

const users = ref([]);
const selectedUser = ref('');
const posts = ref([]);

const fetchUsers = async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await response.json();
  users.value = data;
};

const fetchPosts = async () => {
  if (!selectedUser.value) return;
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
  const data = await response.json();
  posts.value = data;
};

onMounted(() => {
  fetchUsers();
});
</script>

<template>
  <div class="post-container">
    <h2>Post</h2>
    <select v-model="selectedUser" @change="fetchPosts" class="select-user">
      <option value="">Select User</option>
      <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
    </select>
    <ul v-if="posts.length" class="post-list">
      <li v-for="post in posts" :key="post.id" class="post-item">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
      </li>
    </ul>
    <p v-else-if="selectedUser" class="no-posts">No posts available for this user</p>
    <p v-else class="no-posts">No posts available</p>
  </div>
</template>

<style scoped>
.post-container {
  padding: 20px;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
}

h2 {
  font-size: 28px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}

.select-user {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 20px;
}

.post-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.post-item {
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 10px;
  margin-bottom: 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, background-color 0.3s;
}

.post-item:hover {
  transform: scale(1.02);
  background-color: #e9ecef;
}

.post-item h3 {
  font-size: 24px;
  font-weight: bold;
  margin: 0 0 10px;
  color: #333;
}

.post-item p {
  font-size: 16px;
  margin: 0;
  color: #555;
}

.no-posts {
  color: #666;
  font-style: italic;
  text-align: center;
}
</style>