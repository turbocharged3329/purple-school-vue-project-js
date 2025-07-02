<script setup>
import WButton from "./components/WButton.vue";
</script>

<template>
<main class="w-main-container">
  <WButton />
</main>
</template>

<style scoped>
.w-main-container {
  width: 67.5vw;
  background-color: var(--color-bg-main);
  box-sizing: border-box;
  padding: 3.5rem 3.125rem 3.875rem; /* 56px 50px 62px */
  border-radius: 1.5625rem; /* 25px */
}
</style>
