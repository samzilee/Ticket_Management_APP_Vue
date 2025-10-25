<script setup>
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'
</script>

<template>
  <div id="app">
    <Navbar />
    <div class="main-container">
      <router-view />
    </div>
    <Footer />
  </div>
</template>

<style>
.main-container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 0 1rem;
}
</style>
