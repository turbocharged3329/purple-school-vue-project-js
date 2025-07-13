<template>
  <button class="w-button">
    <div class="w-button__icon">
      <slot name="icon"></slot>
    </div>

    <slot></slot>
  </button>
</template>

<script setup>

</script>

<style scoped>
.w-button {
  width: 100%;
  height: 53px;
  border: none;
  border-radius: 10px;
  background: var(--btn-bg-gradient);
  color: var(--color-primary);
  font-size: 20px;
  font-weight: 600;
  padding: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.w-button:hover {
  background: var(--btn-bg-gradient--inverted);
}

.w-button__icon {
  margin-right: .5625rem;
  /* 9px */
}
</style>