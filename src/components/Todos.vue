<script setup>
import { ref, computed } from 'vue';

const newTask = ref("");
const tasks = ref(
  JSON.parse(localStorage.getItem("tasks")) || [
    { text: "Task 1", checked: false },
    { text: "Task 2", checked: true },
  ]
);
const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value ? tasks.value.filter((task) => !task.checked) : tasks.value;
});

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, checked: false });
    newTask.value = "";
    saveData();
  }
}

function removeTask(task) {
  const index = tasks.value.indexOf(task);
  tasks.value.splice(index, 1);
  saveData();
}

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}
</script>

<template>
  <div class="todo-app">
    <h1>Do More</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a task" />
    <button @click="addTask">ADD</button>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? "Show All" : "Hide Completed" }}
    </button>
    <ul>
      <li v-for="(task, index) in filteredTodos" :key="index">
        <input type="checkbox" v-model="task.checked" @change="saveData" />
        <span :class="{ done: task.checked }">{{ task.text }}</span>
        <button @click="removeTask(task)">X</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.todo-app {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: rgb(161, 228, 248);
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 36px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
  box-sizing: border-box;
  margin-bottom: 10px;
}

button {
  background-color: #4caf50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  margin-right: 10px;
}

button:hover {
  background-color: #45a049;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #f5f5f5;
  border-radius: 5px;
  margin-bottom: 5px;
}

li:nth-child(even) {
  background-color: #fafafa;
}

li span {
  flex: 1;
  margin-left: 10px;
  text-decoration: none;
  color: #333;
}

li .remove {
  display: inline-block;
  padding: 5px 10px;
  font-size: 16px;
  border-radius: 5px;
  background-color: #f44336;
  color: white;
  cursor: pointer;
}

li .remove:hover {
  background-color: #da190b;
}

.done {
  text-decoration: line-through;
  color: #ccc;
}
</style>
