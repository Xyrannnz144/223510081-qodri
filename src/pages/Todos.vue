<template>
    <div class="todo-container">
      <h2>Todos</h2>
      <div class="todo-list">
        <div v-for="task in tasks" :key="task.id" class="todo-item">
          <div class="task-info">
            <input type="checkbox" v-model="task.done" class="checkbox" />
            <span :class="{ 'done': task.done }">{{ task.name }}</span>
          </div>
          <button @click="deleteTask(task.id)" class="delete-button">
            <i class="fas fa-trash-alt"></i>
          </button>
        </div>
      </div>
      <form @submit.prevent="addTask" class="add-form">
        <input type="text" v-model="newTask.name" placeholder="Tambahkan tugas baru" class="input-text" />
        <button type="submit" class="add-button">
          <i class="fas fa-plus"></i> Tambah
        </button>
      </form>
      <button @click="showCompletedTasks" class="show-completed-button">
        <i class="fas fa-check"></i> Tampilkan Tugas Selesai
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [
          { id: 1, name: 'Beli paket', done: false },
          { id: 2, name: 'Rapat himatif', done: false },
          { id: 3, name: 'Masuk Kuliah', done: false }
        ],
        newTask: { name: '' }
      };
    },
    methods: {
      addTask() {
        if (this.newTask.name.trim()) {
          this.tasks.push({ id: this.tasks.length + 1, name: this.newTask.name, done: false });
          this.newTask.name = '';
        }
      },
      deleteTask(taskId) {
        this.tasks = this.tasks.filter(task => task.id !== taskId);
      },
      showCompletedTasks() {
        this.tasks = this.tasks.filter(task => task.done);
      }
    }
  };
  </script>
  
  <style scoped>
  .todo-container {
    max-width: 800px;
    margin: 40px auto;
    padding: 30px;
    background-color: #34495e;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    font-family: 'Roboto', sans-serif;
    color: #fff;
  }
  
  h2 {
    text-align: center;
    color: #3498db;
    font-size: 36px;
    margin-bottom: 30px;
    text-transform: uppercase;
  }
  
  .todo-list {
    display: grid;
    gap: 20px;
  }
  
  .todo-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px;
    background-color: #2c3e50;
    border-radius: 12px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .todo-item:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.5);
  }
  
  .task-info {
    display: flex;
    align-items: center;
  }
  
  .checkbox {
    margin-right: 15px;
    width: 25px;
    height: 25px;
    border-radius: 5px;
    cursor: pointer;
    appearance: none;
    background-color: #3498db;
    border: none;
    transition: background-color 0.3s;
  }
  
  .checkbox:checked {
    background-color: #27ae60;
  }
  
  span {
    font-size: 20px;
    color: #ecf0f1;
  }
  
  .done {
    text-decoration: line-through;
    color: #bdc3c7;
  }
  
  .delete-button {
    background: none;
    border: none;
    cursor: pointer;
    font-size: 20px;
    color: #e74c3c;
    transition: color 0.3s;
  }
  
  .delete-button:hover {
    color: #c0392b;
  }
  
  .add-form {
    display: flex;
    align-items: center;
    margin-top: 30px;
  }
  
  .input-text {
    flex: 1;
    padding: 15px;
    border: 2px solid #bdc3c7;
    border-radius: 8px;
    margin-right: 15px;
    font-size: 18px;
    color: #333;
    background-color: #2ecc71;
    outline: none;
  }
  
  .input-text::placeholder {
    color: #bdc3c7;
  }
  
  .add-button {
    padding: 15px 25px;
    background-color: #3498db;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 18px;
  }
  
  .add-button:hover {
    background-color: #2980b9;
  }
  
  .show-completed-button {
    margin-top: 30px;
    padding: 15px 25px;
    background-color: #e74c3c;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    display: block;
    width: 100%;
    text-align: center;
    transition: background-color 0.3s;
    font-size: 18px;
  }
  
  .show-completed-button:hover {
    background-color: #c0392b;
  }
  </style>
  