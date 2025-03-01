<script lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
export default {
  components: {
    Navbar,
  },
}
</script>

<template>
  <header>
    <Navbar />
  </header>
  <RouterView />
</template>

<style scoped></style>
