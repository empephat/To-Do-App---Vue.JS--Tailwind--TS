<script setup lang="ts">
import { ref, computed } from "vue";
import { Star, Trash2, Plus, Moon, Sun } from "lucide-vue-next";

interface Todo {
  id: number;
  text: string;
  completed: boolean;
  priority: boolean;
  description: string;
  date: Date;
}

const isDark = ref(false);
const showError = ref(false);
const newTodoText = ref("");
const todos = ref<Todo[]>([
  {
    id: 1,
    text: "Learn Vue 3",
    completed: true,
    priority: false,
    description: "This is a description",
    date: new Date(),
  },
  {
    id: 2,
    text: "Learn TypeScript",
    completed: false,
    priority: true,
    description: "This is a description",
    date: new Date(),
  },
  {
    id: 3,
    text: "Learn Testing",
    completed: false,
    priority: false,
    description: "This is a description",
    date: new Date(),
  },
]);

const isModalOpen = ref(false);
const editingTodo = ref<Todo | null>(null);

const toggleTheme = () => {
  isDark.value = !isDark.value;
  document.documentElement.classList.toggle("dark");
};

const addTodo = () => {
  if (newTodoText.value.length < 4) {
    showError.value = true;
    return;
  }
  showError.value = false;
  todos.value.push({
    id: Date.now(),
    text: newTodoText.value,
    completed: false,
    priority: false,
    description: "",
    date: new Date(),
  });
  newTodoText.value = "";
};

const toggleComplete = (todo: Todo) => {
  todo.completed = !todo.completed;
};

const togglePriority = (todo: Todo) => {
  todo.priority = !todo.priority;
};

const removeTodo = (todo: Todo) => {
  todos.value = todos.value.filter((t) => t.id !== todo.id);
};

const openModal = (todo: Todo) => {
  editingTodo.value = { ...todo };
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
  editingTodo.value = null;
};

const saveChanges = () => {
  if (editingTodo.value) {
    const index = todos.value.findIndex((t) => t.id === editingTodo.value!.id);
    if (index !== -1) {
      todos.value[index] = { ...editingTodo.value };
    }
  }
  closeModal();
};

const remainingTasks = computed(
  () => todos.value.filter((t) => !t.completed).length
);
const priorityTasks = computed(
  () => todos.value.filter((t) => t.priority).length
);
</script>
<template>
  <div
    class="min-h-screen bg-gray-100 dark:bg-gray-900 flex items-center justify-center p-4"
  >
    <div
      class="w-full max-w-md bg-white dark:bg-gray-800 rounded-lg shadow-xl overflow-hidden"
    >
      <div class="p-6">
        <div class="flex items-center justify-between mb-4">
          <h1 class="text-3xl font-bold text-gray-900 dark:text-white">
            Welcome
          </h1>
          <button
            @click="toggleTheme"
            class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors"
          >
            <Sun
              v-if="isDark"
              class="h-5 w-5 text-gray-600 dark:text-gray-400"
            />
            <Moon v-else class="h-5 w-5 text-gray-600 dark:text-gray-400" />
          </button>
        </div>
        <p class="text-gray-600 dark:text-gray-400 mb-6">
          This is a to-do list app.
        </p>

        <div class="flex space-x-2 mb-4">
          <input
            v-model="newTodoText"
            type="text"
            placeholder="Add A New Todo"
            @keyup.enter="addTodo"
            class="flex-1 px-4 py-2 border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            :class="{ 'border-red-500': showError }"
          />
          <button
            @click="addTodo"
            class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors"
          >
            <Plus class="h-5 w-5" />
          </button>
        </div>
        <p v-if="showError" class="text-red-500 text-sm mb-4">
          Your to-do must be at least 4 characters long.
        </p>

        <h2 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">
          To-Do List:
        </h2>
        <TransitionGroup name="list" tag="ul" class="space-y-2">
          <li
            v-for="todo in todos"
            :key="todo.id"
            class="flex items-center space-x-4 p-4 bg-gray-50 dark:bg-gray-700 rounded-lg transition-all duration-300 ease-in-out"
            :class="{
              'bg-gray-200 dark:bg-gray-600': todo.completed,
              'border-l-4 border-blue-500': todo.priority,
            }"
          >
            <input
              type="checkbox"
              :checked="todo.completed"
              @change="toggleComplete(todo)"
              class="h-5 w-5 text-blue-600 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 hover:cursor-pointer"
            />
            <span
              @click="openModal(todo)"
              class="flex-1 text-gray-800 dark:text-gray-200"
              :class="{
                'line-through text-gray-500 dark:text-gray-400': todo.completed,
              }"
            >
              {{ todo.text }}
            </span>
            <span
              class="text-xs text-gray-500 dark:text-gray-400 ml-2 flex-shrink-0"
            >
              {{ new Date(todo.date).toLocaleDateString("en-SE") }}
            </span>

            <div class="flex space-x-2">
              <button
                @click="togglePriority(todo)"
                class="p-1 rounded-full hover:bg-gray-200 dark:hover:bg-gray-600 transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500"
                :class="{ 'text-yellow-500': todo.priority }"
              >
                <Star class="h-5 w-5" />
              </button>
              <button
                @click="removeTodo(todo)"
                class="p-1 rounded-full hover:bg-gray-200 dark:hover:bg-gray-600 transition-colors focus:outline-none focus:ring-2 focus:ring-red-500 text-red-500"
              >
                <Trash2 class="h-5 w-5" />
              </button>
            </div>
          </li>
        </TransitionGroup>
      </div>
      <div
        class="px-6 py-4 bg-gray-50 dark:bg-gray-700 flex justify-between text-sm text-gray-600 dark:text-gray-400"
      >
        <p>{{ remainingTasks }} tasks remaining</p>
        <p>{{ priorityTasks }} priority tasks</p>
      </div>
    </div>
  </div>

  <!-- Modal -->
  <Transition name="modal">
    <div
      v-if="isModalOpen && editingTodo"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4"
    >
      <div
        class="bg-white dark:bg-gray-800 rounded-lg shadow-xl max-w-md w-full"
      >
        <div class="p-6">
          <input
            v-model="editingTodo.text"
            class="text-2xl font-bold mb-4 w-full bg-transparent text-gray-900 dark:text-white border-b border-gray-300 dark:border-gray-600 focus:outline-none focus:border-blue-500"
          />
          <textarea
            v-model="editingTodo.description"
            class="w-full h-24 p-2 mb-4 text-gray-600 dark:text-gray-400 bg-gray-100 dark:bg-gray-700 rounded-md resize-none focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Add a description..."
          ></textarea>
          <div class="flex items-center space-x-4 mb-4">
            <span class="text-gray-600 dark:text-gray-400">Status:</span>
            <span
              :class="
                editingTodo.completed ? 'text-green-500' : 'text-yellow-500'
              "
            >
              {{ editingTodo.completed ? "Completed" : "Pending" }}
            </span>
          </div>
          <div class="flex items-center space-x-4 mb-4">
            <span class="text-gray-600 dark:text-gray-400">Priority:</span>
            <span
              :class="editingTodo.priority ? 'text-red-500' : 'text-blue-500'"
            >
              {{ editingTodo.priority ? "High" : "Normal" }}
            </span>
          </div>
          <div class="flex items-center space-x-4 mb-4">
            <span class="text-gray-600 dark:text-gray-400">Date:</span>
            <input
              type="date"
              v-model="editingTodo.date"
              class="px-2 py-1 border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            />
          </div>
          <div class="flex justify-end space-x-2">
            <button
              @click="closeModal"
              class="px-4 py-2 bg-gray-300 text-gray-700 rounded-md hover:bg-gray-400 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 transition-colors"
            >
              Cancel
            </button>
            <button
              @click="saveChanges"
              class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors"
            >
              Save Changes
            </button>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
