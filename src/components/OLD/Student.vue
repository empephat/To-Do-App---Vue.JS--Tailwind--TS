<script setup lang="ts">
import { ref } from "vue";
const name = "Emil";
const age = 25;
const isStudent = true;
let studytime = ref(0);

function increaseStudytime() {
  studytime.value += 1;
}
function decreaseStudytime() {
  studytime.value -= 1;
}
</script>
<template>
  <p class="text">
    {{ name }} is {{ age }} years old and
    {{ isStudent ? "is a student" : "is not a student" }}
  </p>
  <p class="text">
    This is how many years {{ name }} has been coding: {{ studytime }}
  </p>
  <div class="container">
    <button @click="increaseStudytime()" class="button">
      Increase studytime
    </button>
    <button @click="decreaseStudytime()" class="button">
      Decrease studytime
    </button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}
.text {
  text-align: center;
}
.button {
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  background-color: var(--color-background-soft);
  color: var(--color-text);
  border: 1px solid var(--color-border);
  cursor: pointer;
  transition: 0.3s;
  margin-left: 1rem;
}

.button:hover {
  background-color: var(--color-border);
  color: var(--color-background);
}
</style>
