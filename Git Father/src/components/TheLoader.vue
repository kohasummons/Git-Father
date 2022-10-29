<script setup>
import SearchIcon from "./icons/IconLoader.vue";
defineProps({
  showIcon: Boolean,
});
</script>
<template>
  <div class="c-loader__container">
    <SearchIcon :class="{ isVisible: showIcon }" class="c-icon" />
    <div class="c-loader"></div>
  </div>
</template>
<style scoped>
.c-loader__container {
  display: flex;
  position: relative;
  justify-content: center;
}

.c-icon {
  display: none;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
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

.isVisible {
  display: block;
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
