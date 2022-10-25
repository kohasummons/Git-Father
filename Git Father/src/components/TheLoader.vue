<script setup></script>
<template>
  <div class="c-loader__container">
    <div class="c-loader"></div>
  </div>
</template>
<style scoped>
.c-loader__container {
  display: flex;
  justify-content: center;
  margin-top: 12rem;
}

.c-loader {
  padding: 2em;
  border: 4px solid var(--color-theme);
  width: 2em;
  height: auto;
  border-radius: 50%;
  border-left-color: transparent;
  animation: spin 1s forwards infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
