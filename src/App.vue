<template>
  <main class="w-main-container">

    <WLeftSideComponent />
  </main>
</template>

<script setup>
import WLeftSideComponent from "./components/WLeftSideComponent.vue";
</script>

<style scoped>
.w-main-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
