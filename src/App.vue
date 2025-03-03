<script lang="ts">
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
