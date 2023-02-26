<script setup lang="ts">
import Blackjack from './components/Blackjack.vue'
import NavB from './components/NavBar.vue'
import foot from './components/Footer.vue'
</script>

<template>  
  <NavB/>
  <Blackjack/>
  <foot/>
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
