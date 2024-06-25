<script setup>
import { ref, computed } from 'vue';

const newTask = ref("");
const tasks = ref(
  JSON.parse(localStorage.getItem("tasks")) || [
    { text: "Task 1", checked: false, isEditing: false },
    { text: "Task 2", checked: true, isEditing: false },
  ]
);
const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value ? tasks.value.filter((task) => !task.checked) : tasks.value;
});

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, checked: false, isEditing: false });
    newTask.value = "";
    saveData();
  }
}

function removeTask(task) {
  const index = tasks.value.indexOf(task);
  tasks.value.splice(index, 1);
  saveData();
}

function toggleEditTask(task) {
  task.isEditing = !task.isEditing;
  saveData();
}

function updateTask(task, newText) {
  task.text = newText;
  task.isEditing = false;
  saveData();
}

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}
</script>

<template>
  <div class="todo-app">
    <h1>Do More</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a task" class="task-input" />
    <div class="button-group">
      <button @click="addTask" class="primary-button">ADD</button>
      <button @click="hideCompleted = !hideCompleted" class="secondary-button">
        {{ hideCompleted ? "Show All" : "Hide Completed" }}
      </button>
    </div>
    <ul class="task-list">
      <li v-for="(task, index) in filteredTodos" :key="index" class="task-item">
        <input type="checkbox" v-model="task.checked" @change="saveData" />
        <div v-if="task.isEditing" class="edit-container">
          <input 
            v-model="task.text" 
            @keyup.enter="updateTask(task, task.text)" 
            class="edit-input" 
          />
          <button @click="updateTask(task, task.text)" class="save-button">Save</button>
          <button @click="toggleEditTask(task)" class="cancel-button">Cancel</button>
        </div>
        <div v-else class="task-view">
          <span :class="{ done: task.checked }">{{ task.text }}</span>
          <button @click="toggleEditTask(task)" class="edit-button">
            <span class="material-icons">edit</span>
          </button>
          <button @click="removeTask(task)" class="remove-button">
            <span class="material-icons">delete</span>
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');

.todo-app {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: #ffffff;
  border-radius: 10px;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 32px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}

.task-input {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
}

.button-group {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.primary-button, .secondary-button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s;
}

.primary-button {
  background-color: #28a745;
  color: white;
}

.primary-button:hover {
  background-color: #218838;
  transform: scale(1.05);
}

.secondary-button {
  background-color: #ffc107;
  color: white;
}

.secondary-button:hover {
  background-color: #e0a800;
  transform: scale(1.05);
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 5px;
  margin-bottom: 10px;
  transition: transform 0.2s, background-color 0.3s;
}

.task-item:nth-child(even) {
  background-color: #e9ecef;
}

.task-item:hover {
  transform: scale(1.02);
}

.task-item input[type="checkbox"] {
  margin-right: 10px;
}

.task-view {
  display: flex;
  align-items: center;
  width: 100%;
}

.task-view span {
  flex: 1;
  font-size: 16px;
}

.edit-container {
  display: flex;
  align-items: center;
  width: 100%;
}

.edit-input {
  flex: 1;
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.done {
  text-decoration: line-through;
  color: #ccc;
}

.edit-button, .remove-button, .save-button, .cancel-button {
  padding: 5px 10px;
  font-size: 14px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s;
}

.edit-button {
  background-color: #007bff;
  color: white;
  margin-right: 5px;
}

.edit-button:hover {
  background-color: #0056b3;
  transform: scale(1.1);
}

.remove-button {
  background-color: #dc3545;
  color: white;
}

.remove-button:hover {
  background-color: #c82333;
  transform: scale(1.1);
}

.save-button {
  background-color: #28a745;
  color: white;
  margin-right: 5px;
}

.save-button:hover {
  background-color: #218838;
  transform: scale(1.1);
}

.cancel-button {
  background-color: #6c757d;
  color: white;
}

.cancel-button:hover {
  background-color: #5a6268;
  transform: scale(1.1);
}
</style>