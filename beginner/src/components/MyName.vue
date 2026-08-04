<template>
  <h1 class="name">My name is {{ name }}</h1>
  <a :href="githubLink">{{ name }} Github link.</a>
</template>

<script setup>
import { ref } from 'vue'

const name = ref('Roger')
const githubLink = ref('https://github.com/techboyRoger7')
</script>

<style scoped>
.name {
  color: aqua;
}
</style>
