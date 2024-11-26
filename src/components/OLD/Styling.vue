<script setup>
const dynamic = "active";
</script>

<template>
  <p class="static" :class="dynamic">Styling</p>
</template>

<style scoped>
.static {
  margin-top: 2rem;
  color: black;
}
.active {
  background-color: red;
}
</style>
