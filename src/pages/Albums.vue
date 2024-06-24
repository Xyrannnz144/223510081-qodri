<template>
    <div class="container">
      <h1 class="page-title">My Albums</h1>
      <div class="album-grid">
        <div v-for="album in albums" :key="album.id" class="album-card" @click="goToAlbum(album.id)">
          <div class="album-header" :style="{ backgroundImage: 'url(' + getAlbumCover(album.id) + ')' }">
            <div class="overlay">
              <div class="album-title">{{ album.title }}</div>
            </div>
          </div>
          <div class="album-footer">
            <q-icon name="fas fa-images" size="18px" class="album-icon"></q-icon>
            <span>{{ album.photos.length }} Photos</span>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'Albums',
    setup() {
      const albums = ref([]);
      const router = useRouter();
  
      const fetchAlbums = async () => {
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/albums');
          albums.value = response.data.map(album => ({ ...album, photos: [] })); // Placeholder for photos
        } catch (error) {
          console.error('Error fetching albums:', error);
        }
      };
  
      const goToAlbum = (id) => {
        router.push({ name: 'AlbumPhotos', params: { id } });
      };
  
      const getAlbumCover = (albumId) => {
        // Placeholder function to generate dynamic album covers
        return `https://picsum.photos/seed/${albumId}/300/200`;
      };
  
      onMounted(fetchAlbums);
  
      return {
        albums,
        goToAlbum,
        getAlbumCover,
      };
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 1000px;
    margin: 50px auto;
    padding: 20px;
    font-family: 'Roboto', sans-serif;
    color: #333;
  }
  
  .page-title {
    font-size: 36px;
    text-align: center;
    margin-bottom: 30px;
  }
  
  .album-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
  }
  
  .album-card {
    position: relative;
    overflow: hidden;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    cursor: pointer;
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .album-card:hover {
    transform: translateY(-10px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.3);
  }
  
  .album-header {
    height: 200px;
    background-size: cover;
    background-position: center;
    position: relative;
  }
  
  .overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 10px;
  }
  
  .album-title {
    font-size: 24px;
    color: #fff;
  }
  
  .album-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px;
    background-color: #3498db;
    color: #fff;
    border-bottom-left-radius: 12px;
    border-bottom-right-radius: 12px;
  }
  
  .album-icon {
    margin-right: 5px;
  }
  
  @media (max-width: 768px) {
    .album-card {
      width: calc(50% - 10px);
    }
  }
  </style>