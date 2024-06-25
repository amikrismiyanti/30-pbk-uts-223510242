<template>
    <div class="album">
      <div v-for="photo in photos" :key="photo.id" class="photo-item">
        <img :src="photo.url" :alt="photo.title">
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        photos: []
      };
    },
    mounted() {
      axios.get('https://jsonplaceholder.typicode.com/photos')
        .then(response => {
          this.photos = response.data;
        })
        .catch(error => {
          console.error('Error fetching photos:', error);
        });
    }
  };
  </script>
  
  <style scoped>
  .album {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .photo-item {
    width: 200px;
    height: 200px;
    margin: 10px;
    overflow: hidden;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
  }
  
  .photo-item:hover {
    transform: scale(1.05);
  }
  
  .photo-item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 8px;
  }
  </style>
  