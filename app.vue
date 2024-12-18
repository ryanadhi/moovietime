<template>
  <div id="app" class="flex flex-col h-screen overflow-hidden">
    <!-- Navbar stays fixed at the top -->
    <Navbar />

    <!-- Main Content -->
    <div class="main-content flex-1 overflow-y-auto">
      <NuxtLayout>
        <NuxtPage />
      </NuxtLayout>
    </div>

    <!-- Footer stays at the bottom -->
    <Footer />
  </div>
</template>

<script setup>
import Navbar from '~/components/Navbar.vue'
import Footer from '~/components/Footer.vue'
</script>

<style>
</style>