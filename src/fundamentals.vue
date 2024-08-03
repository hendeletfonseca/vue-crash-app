<script setup>
import { ref , onMounted} from 'vue'

const name = ref('Hendel Fonseca');
const status = ref('active');
const tasks = ref(['Task One', 'Task Two']);
const newTask = ref('');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending';
  } else if (status.value === 'pending') {
    status.value = 'inactive';
  } else {
    status.value = 'active';
  }
}; 

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const removeTask = (ind) => {
  tasks.value.splice(ind, 1);
}

onMounted(async () => {
  try {
    const response = await fetch('https:jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {

  }
});
</script>

<template>
  <h1>{{ name }}</h1>
  <p>User is {{ status }}</p>

<form @submit.prevent="addTask">
  <label for="newTask">Add Task</label>
  <input type="text" id="newTask" name="newTask" v-model="newTask">
  <button type="submit">Submit</button>  
</form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, ind) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="removeTask(ind)">Remove Task</button>
    </li>
  </ul>
  

  <button @click="toggleStatus">Change Status</button>
</template>