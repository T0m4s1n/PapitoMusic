<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import { useRouter } from "vue-router";
import { onMounted } from "vue";

const router = useRouter();

</script>
<template>
  <RouterView />
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap");

main {
  font-family: "Poppins", sans-serif;
  background-color: #0c0c0c;
  color: #ffffff;
  min-height: 100vh;
  min-width: auto;
  align-items: center;
  justify-content: center;
}
</style>