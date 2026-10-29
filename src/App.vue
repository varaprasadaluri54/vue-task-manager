<template>
  <main class="container">
    <h1> My Task Manager</h1>

    <form @submit.prevent="addTask">
       <input v-model="newTask" placeholder="Enter a task" />
       <button type="submit">Add Task</button>
    </form>

    <select  v-model="filter">
      <option value="all">All Tasks</option>
      <option value="active">Active</option>
      <option value="completed">Completed</option>
    </select>

    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" :checked="task.completed"  @change="toggleTask(task.id)" />
        <span>{{ task.title }}</span>
        <button @click="deleteTask(task.id)">Delete</button>
      </li>
    </ul>

  </main>
</template>
<script setup>
import { ref, computed } from "vue"

const newTask = ref('')
const tasks = ref([])
const filter = ref('all')

function addTask() {
  if (newTask.value.trim() != '') {
    tasks.value.push({
      id: Date.now(),
      title: newTask.value.trim(),
      completed: false
    })
    newTask.value = ''
   }
}

function toggleTask(id) {
  const task = tasks.value.find(task => task.id === id)
  if (task) {
    task.completed = !task.completed
  }
}
function deleteTask(id) {
  const filtered = tasks.value.filter((it) => it.id !== id)
   tasks.value = filtered
}

const filteredTasks = computed(() => {
  if (filter.value === 'active') {
    return tasks.value.filter((it)=> !it.completed)
  } else if (filter.value === 'completed') {
    return tasks.value.filter((it)=> it.completed)
  } else {
    return tasks.value
  }
})
</script>
<style>
.container{
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
}

form{
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}
form input{
  flex: 1;
  padding:10px;
}
button, select{
  padding:8px 12px;
  cursor: pointer;
}
li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 12px 0;
}
li span{
  flex: 1;
}
</style>