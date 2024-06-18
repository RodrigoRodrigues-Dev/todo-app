<template>
    <div>
      <h2>Task List</h2>
      <AddTask @task-added="fetchTasks" />
      <ul>
        <TaskItem
          v-for="task in tasks"
          :key="task._id"
          :task="task"
          @task-updated="fetchTasks"
          @task-deleted="fetchTasks"
        />
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import TaskItem from './TaskItem.vue';
  import AddTask from './AddTask.vue';
  
  export default {
    name: 'TaskList',
    components: {
      TaskItem,
      AddTask,
    },
    data() {
      return {
        tasks: [],
      };
    },
    methods: {
      async fetchTasks() {
        const token = localStorage.getItem('token');
        if (token) {
          try {
            const response = await axios.get('/tasks', {
              headers: {
                Authorization: `Bearer ${token}`,
              },
            });
            this.tasks = response.data;
          } catch (error) {
            console.error(error);
          }
        }
      },
    },
    created() {
      this.fetchTasks();
    },
  };
  </script>  