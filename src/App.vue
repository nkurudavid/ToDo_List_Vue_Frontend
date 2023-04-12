<script setup>
  import { RouterLink, RouterView } from 'vue-router'
  import Navbar from './components/Navbar.vue'
  import Footer from './components/Footer.vue'
</script>

<template>

  <Navbar />

  <RouterView :class="'mb-5 mt-5'" />

  <Footer />

</template>

