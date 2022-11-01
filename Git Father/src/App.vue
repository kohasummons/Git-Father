<script setup>
import { reactive, computed, watch } from "vue";
import ToolingIcon from "./components/icons/IconTooling.vue";
import TheLoader from "./components/TheLoader.vue";
// import TestAxiosVue from "./components/TestAxios.vue";
import axios from "axios";

const JOKES_API =
  "https://official-joke-api.appspot.com/jokes/programming/random";
const RANDOM_WORD_API = "https://api.api-ninjas.com/v1/randomword";

const data = reactive({
  isLoading: false,
  userName: "iyki",
  user: null,
  joke: null,
  hideImageLoader: false,
  random1: null,
  random2: null,
  random3: null,
});

const firstName = computed(() => {
  if (!data.user?.name) return data.user?.login;
  return data.user?.name.split(" ")[0];
});

const fullName = computed(() => {
  return data.user?.name || "You need a cool name";
});

const getJoke = async () => {
  let { data: joke } = await axios.get(JOKES_API);
  return joke;
};

const getRandomWord = async () => {
  let randomword = await axios.get(RANDOM_WORD_API);
  return randomword.data.word;
};

watch(
  () => data.user,
  async () => {
    if (data.user.avatarUrl) {
      setTimeout(() => (data.hideImageLoader = data.isLoading), 2000);
    }
  }
);

const getUser = async () => {
  if (data.userName == "") return;
  data.isLoading = true;

  const query = {
    query: `
        query {
          user(login: "${data.userName}") {
            name
            login
            bio
            avatarUrl
            status {
              message
            }
            url
            isFollowingViewer
          }
        }
      `,
  };
  data.joke = await getJoke();
  data.random1 = await getRandomWord();
  data.random2 = await getRandomWord();
  data.random3 = await getRandomWord();

  try {
    let { data: foundUser } = await axios({
      method: "POST",
      url: "https://api.github.com/graphql",
      headers: {
        "content-type": "application/json",
        Authorization: `bearer ${import.meta.env.VITE_GITHUB_TOKEN}`,
      },
      data: query,
    });

    data.user = foundUser.data.user;
    data.isLoading = false;
  } catch (error) {
    data.isLoading = false;
    data.showError = true;
  }
};
</script>

<template>
  <div class="c-main">
    <!-- <TestAxiosVue /> -->
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
    <section v-if="!data.user" class="c-section c-section-ux">
      <div class="c-section__inner">
        <h3>
          Hey! Start by Entering your Github User Name and I will tell you a
          joke
        </h3>
      </div>
    </section>
    <section v-show="!data.isLoading && data.user" class="c-section">
      <div class="c-card__header">
        <div class="c-card__image">
          <img
            class="c-image"
            @load="data.hideImageLoader = true"
            :src="data.user?.avatarUrl"
          />
          <TheLoader v-if="data.hideImageLoader" class="loader" />
        </div>
        <div class="c-card__details">
          <div class="c-card__user">
            <h2 class="c-name">
              <a :href="data.user?.url">{{ fullName }}</a>
            </h2>
            <p class="c-username">@{{ data.user?.login }}</p>
          </div>
          <div class="c-stats">
            <span>{{ data.random1 }}</span>
            <span>{{ data.random2 }}</span>
            <span>{{ data.random3 }}</span>
          </div>
        </div>
      </div>
      <div class="c-father">
        <p>
          <span class="c-login">Heyy {{ firstName }}, here's a treat 🤩</span>
        </p>

        <p class="setup">{{ data.joke ? data.joke[0].setup : "" }}</p>
        <p class="punchline">{{ data.joke ? data.joke[0].punchline : "" }}</p>
      </div>
    </section>
    <footer>
      <a href="https://github.com/kohasummons"
        ><p class="c-author">Joshua Omobola</p></a
      >
    </footer>
  </div>
</template>

<style scoped>
header {
  display: flex;
  align-items: center;
}

.c-logo {
  font-size: 2.75em;
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
  background-color: #935e2d;
  padding: 1rem;
  display: flex;
  width: 100%;
}

.c-card__image {
  width: 200px;
  height: 200px;
  position: relative;
  background-color: black;
  border-radius: calc(0.75em / 2);
  background-size: contain;
}

.c-image {
  width: 200px;
  height: auto;
  border-radius: calc(0.75em / 2);
}

.c-card__details {
  padding-left: 2rem;
  padding-top: 1em;
  display: flex;
  flex-direction: column;
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
  color: rgb(202, 143, 24);
}

.c-father p:first-of-type {
  margin-bottom: 0.15em;
}

.loader {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.c-father .setup {
  font-size: 3.25em;
  text-align: left;
}

.c-father .punchline {
  margin-top: 0.75em;
  font-size: 1.75em;
  opacity: 0.5;
}

footer {
  margin-top: 6.5em;
  display: flex;
  justify-content: center;
}

@media screen and (max-width: 900px) {
  #app {
    max-width: 100vw;
  }

  header {
    align-items: flex-start;
    flex-direction: column;
  }

  .c-logo {
    margin-bottom: 0.75em;
    letter-spacing: -2.5px;
  }

  .c-input__wrapper {
    margin-left: unset;
    width: 100%;
  }

  .c-input {
    width: 100%;
  }

  .c-card__header {
    margin-top: 4em;
    width: 100%;
    flex-direction: column;
  }

  .c-card__image {
    width: 500px;
    max-width: 100%;
    height: auto;
  }

  .c-image {
    width: 100%;
    height: auto;
  }

  .c-stats {
    flex-direction: column;
  }

  .c-stats > span {
    display: inline-block;
    font-size: 0.75em;
  }

  .c-card__details {
    padding-left: unset;
  }

  .c-father p:first-of-type {
    margin-bottom: 0.75em;
    font-size: 1.5em;
  }

  .c-father .setup {
    font-size: 2.25em;
    line-height: 1;
  }
}
</style>
