<template>
    <div>
      <input type="text" v-model="title" placeholder="Add new task" />
      <button @click="addTask">Add</button>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'AddTask',
    data() {
      return {
        title: '',
      };
    },
    methods: {
      async addTask() {
        const token = localStorage.getItem('token');
        if (token && this.title) {
          try {
            await axios.post(
              '/tasks',
              { title: this.title },
              {
                headers: {
                  Authorization: `Bearer ${token}`,
                },
              }
            );
            this.title = '';
            this.$emit('task-added');
          } catch (error) {
            console.error(error);
          }
        }
      },
    },
  };
  </script>  