<script setup>

  import { ref, onMounted} from 'vue';

 
      const name = ref('Diogo')
      let status = ref('active')
      const tasks = ref(['Task One', 'Task two', 'Task three'])
      const newTask = ref('')

      const toggleStatus = () => {
        if(status.value === 'active') {
        status.value = 'pending'
       } else if(status.value === 'pending') {
        status.value = 'inactive'
       } else {
        status.value = 'active'
       }
       console.log(status.value);
      }

      const addTask = () => {
        if(newTask.value.trim() !== '') {
          tasks.value.push(newTask.value);
          newTask.value = ''
        }
      }

      const deleteTask = (i) => {
      tasks.value.splice(i, 1);
      }

    onMounted(async () => {

      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos')
        const data = await response.json();
        tasks.value = data.map((task) => task.title);
      }catch(error) {
        console.log(error);
      }

    });
  
</script>

<template>
  <h1>Hello {{ name }}</h1>
  <p v-if="status === 'active'">User is {{status}}</p>
  <p v-if="status === 'pending'">User is {{ status }}</p>
  <p v-if="status === 'inactive'">User is {{ status }}</p>

  <h3>Form</h3>
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>


  <br>
  <br>

  <!-- Task List -->
  <h1>Lists</h1>
  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>

  <h3>Links</h3>
  <a :href="link">Go to google</a>

  <button @click="toggleStatus">Change status</button>
</template>



<style scoped>

</style>