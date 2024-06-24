<template>
    <q-page class="album-photos-page">
      <h1 class="page-title">Foto Album</h1>
      <div class="photos-container">
        <div v-for="photo in photos" :key="photo.id" class="photo-item">
          <div class="photo-thumbnail" @click="viewPhoto(photo.url)">
            <img :src="photo.thumbnailUrl" alt="Thumbnail" class="thumbnail-img" />
          </div>
          <div class="photo-details">
            <p class="photo-title">{{ photo.title }}</p>
          </div>
        </div>
      </div>
      <q-dialog v-model="isPhotoDialogOpen" persistent class="photo-dialog">
        <img :src="currentPhotoUrl" alt="Dialog Photo" class="dialog-photo-img" />
      </q-dialog>
    </q-page>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  
  export default {
    name: 'AlbumPhotos',
    props: {
      id: {
        type: Number,
        required: true
      }
    },
    setup(props) {
      const photos = ref([]);
      const isPhotoDialogOpen = ref(false);
      const currentPhotoUrl = ref('');
  
      const fetchPhotos = async () => {
        try {
          const response = await axios.get(`https://jsonplaceholder.typicode.com/photos?albumId=${props.id}`);
          photos.value = response.data;
        } catch (error) {
          console.error('Error fetching photos:', error);
        }
      };
  
      onMounted(fetchPhotos);
  
      const viewPhoto = (url) => {
        currentPhotoUrl.value = url;
        isPhotoDialogOpen.value = true;
      };
  
      return {
        photos,
        isPhotoDialogOpen,
        currentPhotoUrl,
        viewPhoto
      };
    }
  };
  </script>
  
  <style scoped>
  .album-photos-page {
    padding: 20px;
  }
  
  .page-title {
    font-size: 28px;
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  .photos-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 20px;
  }
  
  .photo-item {
    background-color: #f0f0f0;
    padding: 10px;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .photo-item:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
  }
  
  .photo-thumbnail {
    cursor: pointer;
    overflow: hidden;
    border-radius: 12px;
  }
  
  .thumbnail-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
  }
  
  .photo-details {
    padding-top: 8px;
    text-align: center;
  }
  
  .photo-title {
    font-size: 14px;
    color: #444;
  }
  
  .photo-dialog {
    width: 90%;
    max-width: 600px;
  }
  
  .dialog-photo-img {
    width: 100%;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
  }
  </style>