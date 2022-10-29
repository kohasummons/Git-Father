<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
const result = ref("");

const call = async () => {
  let { data: data } = await axios({
    method: "POST",
    url: "https://api.github.com/graphql",
    headers: {
      "content-type": "application/json",
      Authorization: `bearer ${import.meta.env.VITE_GITHUB_TOKEN}`,
    },
    data: {
      query: `
        query {
            viewer {
                login
            }
        }
      `,
    },
  });
  result.value = data;
};

onMounted(() => {
  call();
});
</script>
<template>
  <div style="color: white">{{ result }}</div>
</template>

<style scoped></style>
