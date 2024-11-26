<script setup lang="ts">
import VueFeather from "vue-feather";
import { ref, computed } from "vue";

const isDarkMode = ref(localStorage.getItem("dark-mode") === "enabled");
const showError = ref(false);
const user = ref("");
const newTodoText = ref("");
const todos = ref([
  { id: 1, text: "Learn Vue 3", completed: true, priority: false },
  { id: 2, text: "Learn TypeScript", completed: false, priority: true },
  { id: 3, text: "Learn Testing", completed: false, priority: false },
]);

const isValidTodo = computed(() => newTodoText.value.length >= 4);

const addTodo = () => {
  if (!isValidTodo.value) {
    showError.value = true;
    return;
  }

  showError.value = false;

  todos.value.push({
    id: Date.now(),
    text: newTodoText.value,
    completed: false,
  });
  newTodoText.value = "";
};

const removeTodo = (todo: { id: number }) => {
  todos.value = todos.value.filter((t) => t.id !== todo.id);
};

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
  localStorage.setItem("dark-mode", isDarkMode.value ? "enabled" : "disabled");
};

const toggleComplete = (todo: { completed: boolean }) => {
  todo.completed = !todo.completed;
};

const togglePriority = (todo: { priority: boolean }) => {
  todo.priority = !todo.priority;
};
</script>

<template>
  <div :class="['app-container', { 'dark-mode': isDarkMode }]">
    <header>
      <h1 class="header-title">Welcome {{ user }}</h1>
      <button class="theme-btn" @click="toggleTheme()">
        <vue-feather
          v-if="isDarkMode"
          type="moon"
          size="16"
          class="filled-icon-dark"
        />
        <vue-feather v-else type="moon" size="16" class="outlined-icon" />
      </button>
      <p class="header-text">This is a to-do list app.</p>
    </header>
    <div class="add-todo">
      <input
        type="text"
        v-model="newTodoText"
        placeholder="Add A New Todo"
        :class="{ error: showError }"
      />
      <button @click="addTodo" class="addbtn">Add New</button>
    </div>
    <p v-if="showError" class="error-message">
      Your to-do must be at least 4 characters long.
    </p>
    <h2>To-Do List:</h2>
    <ul class="todo-list">
      <li
        v-for="todo in todos"
        :key="todo.id"
        :class="{ 'todo-item': true, completed: todo.completed }"
        @click="toggleComplete(todo)"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span :class="{ done: todo.completed }">
          {{ todo.text }}
        </span>
        <div class="todo-actions">
          <button class="priority-btn" @click.stop="togglePriority(todo)">
            <vue-feather
              v-if="todo.priority"
              type="star"
              size="16"
              class="filled-icon"
            />
            <vue-feather v-else type="star" size="16" class="outlined-icon" />
          </button>
          <button class="delete-btn" @click.stop="removeTodo(todo)">
            <vue-feather type="x" size="16" />
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style>
.app-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #ffffff;
  color: #000000;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.app-container.dark-mode {
  background-color: #1e1e1e;
  color: #ffffff;
}

header {
  padding: 20px;
  background-color: #f0f0f0;
  color: black;
  text-align: center;
  border-radius: 8px;
  margin-bottom: 20px;
}

header.dark-mode {
  background-color: #333;
  color: white;
}
.header-title {
  padding-bottom: 1rem;
}

.header-text {
  padding-top: 1rem;
}

.theme-btn {
  padding: 8px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  background: white;
  transition: transform 0.3s ease;
}

.theme-btn:hover {
  transform: scale(1.2);
}

.filled-icon {
  color: gold;
}

.filled-icon-dark {
  color: rgb(0, 0, 0);
}

.outlined-icon {
  color: #bbb;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-todo input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: transform 0.3s ease;
}

.add-todo input:hover {
  border: #89ff71 solid 1px;
  transform: scale(1.01);
}

.add-todo input.error {
  border-color: red;
}

.error-message {
  color: red;
  font-size: 0.9em;
  margin-top: -15px;
  margin-bottom: 15px;
}

.stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  background: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background: white;
  color: black;
  border: 1px solid #ddd;
  margin-bottom: 5px;
  border-radius: 4px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.todo-item:hover {
  cursor: pointer;
  transform: scale(1.02);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  border: #89ff71 solid 1px;
}

.addbtn {
  transition: transform 0.3s ease;
}

.addbtn:hover {
  transform: scale(1.1);
}

.todo-item.completed {
  background: #bcbcbc;
}

.todo-item.priority {
  border-left: 4px solid gold;
}

.done {
  text-decoration: line-through;
  color: #888;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  background: #4caf50;
  color: white;
  cursor: pointer;
}

button:disabled {
  background: #cccccc;
  cursor: not-allowed;
}

.todo-actions {
  display: flex;
  gap: 8px;
  margin-left: auto;
}

.priority-btn:hover {
  background-color: #419544;
}

.delete-btn {
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
}

.delete-btn {
  background-color: #ff4444;
}

.delete-btn:hover {
  background-color: #cc0000;
}
</style>
