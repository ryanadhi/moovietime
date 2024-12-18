<template>
  <div id="app" class="flex flex-col min-h-screen">
    <Navbar />
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
    <Footer />
  </div>
</template>


<script setup>
import Navbar from '~/components/Navbar.vue'
import Footer from '~/components/Footer.vue'
</script>

<style>
/* Ensure the content pushes the footer to the bottom */
#app {
  display: flex;
  flex-direction: column;
}
</style>