<template>
    <div class="custom-container">
      <h2>Latest Posts</h2>
      <div class="filter-section">
        <label for="userSelect">Filter by User:</label>
        <select id="userSelect" v-model="selectedUserId" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div class="posts-grid" v-if="posts.length > 0">
        <div v-for="post in posts" :key="post.id" class="post-card">
          <div class="post-header">
            <h3>{{ post.title }}</h3>
          </div>
          <div class="post-body">
            <p>{{ post.body }}</p>
          </div>
          <div class="post-footer">
            <button @click="deletePost(post.id)" class="delete-button">Delete</button>
          </div>
        </div>
      </div>
      <p v-else class="no-posts">No posts found.</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUserId: null,
        posts: []
      };
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          const data = await response.json();
          this.users = data;
          if (this.users.length > 0) {
            this.selectedUserId = this.users[0].id;
            this.fetchPosts();
          }
        } catch (error) {
          console.error('Error fetching users:', error);
        }
      },
      async fetchPosts() {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`);
          const data = await response.json();
          this.posts = data;
        } catch (error) {
          console.error('Error fetching posts:', error);
        }
      },
      async deletePost(postId) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
            method: 'DELETE'
          });
          this.posts = this.posts.filter(post => post.id !== postId);
        } catch (error) {
          console.error('Error deleting post:', error);
        }
      }
    },
    mounted() {
      this.fetchUsers();
    }
  };
  </script>
  
  <style scoped>
  .custom-container {
    max-width: 1000px;
    margin: 50px auto;
    padding: 40px;
    background-color: #8b2978;
    border-radius: 15px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    font-family: 'Arial', sans-serif;
  }
  
  h2 {
    font-size: 36px;
    text-align: center;
    color: #333;
    margin-bottom: 30px;
  }
  
  .filter-section {
    text-align: center;
    margin-bottom: 20px;
  }
  
  label {
    font-size: 18px;
    color: #555;
    margin-right: 10px;
  }
  
  select {
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #fff;
    outline: none;
    transition: border-color 0.3s;
  }
  
  select:hover,
  select:focus {
    border-color: #3498db;
  }
  
  .posts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
  }
  
  .post-card {
    background-color: #fff;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .post-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
  }
  
  .post-header {
    margin-bottom: 10px;
  }
  
  h3 {
    font-size: 24px;
    color: #333;
    margin: 0;
  }
  
  .post-body {
    font-size: 16px;
    color: #555;
  }
  
  .post-footer {
    margin-top: 10px;
    text-align: right;
  }
  
  .delete-button {
    padding: 10px 20px;
    background-color: #e74c3c;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 16px;
  }
  
  .delete-button:hover {
    background-color: #c0392b;
  }
  
  .no-posts {
    text-align: center;
    font-size: 20px;
    color: #555;
    margin-top: 30px;
  }
  </style>