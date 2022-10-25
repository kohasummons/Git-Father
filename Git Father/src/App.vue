<script setup>
import { reactive } from "vue";
import ToolingIcon from "./components/icons/IconTooling.vue";
import TheLoader from "./components/TheLoader.vue";

const data = reactive({
  isLoading: false,
});
</script>

<template>
  <div class="c-main">
    <header>
      <h1 class="c-logo">Git Father</h1>
      <div class="c-input__wrapper">
        <ToolingIcon />
        <input class="c-input" type="text" />
        <button class="c-button">Seek</button>
      </div>
    </header>
    <TheLoader v-if="data.isLoading" />
    <section class="c-section">
      <div class="c-section__inner">
        <h3>
          Hey! Start by Entering any Github User Name and I will reveal Secrets
        </h3>
      </div>
    </section>
  </div>
</template>

<style scoped>
header {
  display: flex;
  align-items: center;
}

.c-logo {
  letter-spacing: -1.5px;
}

.c-input__wrapper {
  padding: 0.5em 1em;
  display: flex;
  align-items: center;
  margin-left: auto;
  background-color: var(--color-background-soft);
}

.c-input {
  height: 2.5rem;
  width: 24rem;
  margin-left: 0.5em;
  border: none;
  outline: none;
  background: none;
  color: var(--color-text);
  font-size: 1.2em;
}

.c-button {
  outline: none;
  border: none;
  padding: 0.95em 1.5em;
  cursor: pointer;
  font-weight: 600;
  color: var(--color-text);
  background-color: #4225c5;
}

.c-button:active {
  background-color: #381bbb;
}

/* Section */
.c-section {
  height: calc(100vh - 200px);
  display: grid;
  place-items: center;
}
.c-section__inner {
  text-align: center;
}
</style>
