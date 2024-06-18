<template>
    <li>
      <span :class="{ completed: task.completed }">{{ task.title }}</span>
      <button @click="toggleComplete">{{ task.completed ? 'Undo' : 'Complete' }}</button>
      <button @click="deleteTask">Delete</button>
    </li>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'TaskItem',
    props: {
      task: Object,
    },
    methods: {
      async toggleComplete() {
        const token = localStorage.getItem('token');
        if (token) {
          try {
            await axios.put(
              `/tasks/${this.task._id}`,
              {
                completed: !this.task.completed,
              },
              {
                headers: {
                  Authorization: `Bearer ${token}`,
                },
              }
            );
            this.$emit('task-updated');
          } catch (error) {
            console.error(error);
          }
        }
      },
      async deleteTask() {
        const token = localStorage.getItem('token');
        if (token) {
          try {
            await axios.delete(`/tasks/${this.task._id}`, {
              headers: {
                Authorization: `Bearer ${token}`,
              },
            });
            this.$emit('task-deleted');
          } catch (error) {
            console.error(error);
          }
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .completed {
    text-decoration: line-through;
  }
  </style>  