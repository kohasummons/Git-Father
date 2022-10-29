<script setup>
import { reactive } from "vue";
import ToolingIcon from "./components/icons/IconTooling.vue";
import TheLoader from "./components/TheLoader.vue";
import TestAxiosVue from "./components/TestAxios.vue";

const data = reactive({
  isLoading: false,
  userName: "",
  foundUser: [],
});

const getUser = () => {
  data.isLoading = !data.isLoading;
};
</script>

<template>
  <div class="c-main">
    <TestAxiosVue />
    <header>
      <h1 class="c-logo">Git Father</h1>
      <div class="c-input__wrapper">
        <ToolingIcon />
        <input
          class="c-input"
          v-model="data.userName"
          @keypress.enter="getUser"
          type="text"
          placeholder="Enter Github Username"
        />
        <button class="c-button" @click="getUser">Seek</button>
      </div>
    </header>
    <TheLoader v-if="data.isLoading" showIcon style="margin-top: 8rem" />
    <section v-if="!data.foundUser" class="c-section c-section-ux">
      <div class="c-section__inner">
        <h3>
          Hey! Start by Entering your Github User Name and I will reveal some
          Secrets
        </h3>
      </div>
    </section>
    <section v-else class="c-section">
      <div class="c-card__header">
        <div
          class="c-card__image"
          style="position: relative; background-color: black"
        >
          <TheLoader
            style="
              position: absolute;
              top: 50%;
              left: 50%;
              transform: translate(-50%, -50%);
            "
          />
        </div>
        <div class="c-card__details">
          <div class="c-card__user">
            <h2 class="c-name">Joshua Omobola</h2>
            <p class="c-username">@kohasummons</p>
          </div>
          <div class="c-stats">
            <span>42 repos</span>
            <span>6 stars</span>
            <span>Gemini</span>
          </div>
        </div>
      </div>
      <div class="c-father">
        <p>
          <span class="c-login">@kanga</span>, Your neigther a fater akdfja
          dkfjaldfj ajdkfa klsdfs sfklsdf sdlsdfs sfjsldfksf sdkflsjfdfsd
          kfslfdksfj
        </p>
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
  border-radius: 0.25em;
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
  color: rgb(173, 215, 3);
  font-size: 1.2em;
}

.c-input::placeholder {
  opacity: 0.45;
}

.c-button {
  outline: none;
  border: none;
  padding: 0.95em 1.5em;
  cursor: pointer;
  font-weight: 600;
  color: var(--color-text);
  border-radius: calc(0.25em / 2);
  background-color: #363635;
}

.c-button:active {
  background-color: #381bbb;
}

/* Section */
.c-section {
  margin-top: 2.5rem;
}
.c-section-ux {
  height: calc(100vh - 500px);
  display: grid;
  place-items: center;
}
.c-section__inner {
  text-align: center;
}

/* Card */
.c-card__header {
  border-radius: 0.75rem;
  background-color: #689033;
  padding: 1rem;
  display: flex;
}

.c-card__image {
  width: 200px;
  height: 200px;
  border-radius: calc(0.75em / 2);
}

.c-card__details {
  padding-left: 2rem;
  padding-top: 1em;
  display: flex;
  flex-direction: column;
}

.c-card__user {
}

.c-name {
  font-size: 3.5rem;
  letter-spacing: -4.5px;
  line-height: 1em;
}
.c-username {
  font-size: 1.75rem;
  font-weight: 300;
  opacity: 0.5;
}

.c-stats {
  font-size: 1.5rem;
  padding-top: 40px;
  /* border: 1px solid red; */
}

.c-stats > * {
  margin-right: 0.75em;
  letter-spacing: -1.15px;
}

.c-father {
  margin-top: 1.5em;
  padding: 1rem;
}

.c-father p {
  font-size: 2.5em;
  line-height: 1.25em;
  letter-spacing: -1.25px;
}

.c-father > p .c-login {
  color: rgb(207, 154, 20);
}
</style>
