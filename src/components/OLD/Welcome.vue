<script setup lang="ts">
import { ref } from "vue";
const msg = "Welcome to Your Vue.js App";
const imageUrl =
  "https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Google_2015_logo.svg/800px-Google_2015_logo.svg.png";
const imageAlt = "Google logo";
const width = 400;
</script>
<template>
  <h1 class="text">{{ msg }}</h1>
  <br />
  <p class="text">
    Vue 3 is here! It's a great way to build modern web apps with less
    boilerplate and no compromises on flexibility.
  </p>
  <br />
  <div class="imageContainer">
    <img class="logo" :src="imageUrl" :alt="imageAlt" :width="width" />
  </div>
  <br />
</template>

<style scoped>
.imageContainer {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}
.text {
  text-align: center;
}
</style>
