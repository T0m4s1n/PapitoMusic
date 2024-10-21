<template>
  <header class="header">
    <div class="logo-container">
      <h1 class="logo">PapitoMusic</h1>
    </div>
    <nav class="nav">
      <ul class="nav-list">
        <li v-for="link in links" :key="link.name" class="nav-item">
          <a href="#" class="nav-link" @click.prevent="navigateTo(link.url)">
            {{ link.name }}
          </a>
        </li>
      </ul>
    </nav>
    <SearchBar />
  </header>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SearchBar from "./SearchBar.vue";

export default defineComponent({
  name: "HeaderComponent",
  components: { SearchBar },
  data() {
    return {
      links: [
        { name: "Home", url: "/Home" },
        { name: "Discover", url: "/Discover" },
        { name: "Playlists", url: "/Playlists" },
        { name: "Profile", url: "/Profile" },
      ],
    };
  },
  methods: {
    navigateTo(url: string) {
      this.$router.push(url);
    },
  },
});
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap");

.header {
  position: fixed;
  top: 35px;
  left: 35px;
  z-index: 1000;
  background-color: #0c0c0c;
  padding: 10px 40px;
  border-radius: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 90%;
  box-shadow: 0px 4px 15px rgba(218, 98, 125, 0.5);
  border-bottom: 2px solid #450920;
  position: relative;
  border-radius: 15px;
}

.logo {
  color: #ffffff;
  font-family: "Poppins", sans-serif;
  font-size: 1.8rem;
  font-weight: 600;
  margin: 0;
  transition: color 0.3s ease;
}

.logo:hover {
  color: #ffa5ab;
}

.nav {
  display: flex;
}

.nav-list {
  list-style: none;
  display: flex;
  gap: 2rem;
  margin: 0;
}

.nav-item {
  position: relative;
}

.nav-link {
  color: #ffffff;
  font-family: "Poppins", sans-serif;
  font-size: 1.1rem;
  text-decoration: none;
  padding-bottom: 4px;
  transition: color 0.3s ease;
}

.nav-link:hover {
  color: #ffa5ab;
}

.nav-link::after {
  content: "";
  position: absolute;
  width: 100%;
  height: 2px;
  background-color: #da627d;
  left: 0;
  bottom: 0;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease;
}

.nav-link:hover::after {
  transform: scaleX(1);
  transform-origin: left;
}

@media (max-width: 768px) {
  .nav-list {
    flex-direction: column;
    gap: 1rem;
  }
}
</style>
