<script setup lang="ts">
import Blackjack from './components/Blackjack.vue'
</script>

<template>
  <h1 class="text-center text-white">Hello World</h1>
  
  <Blackjack/>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
