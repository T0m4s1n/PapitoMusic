<template>
    <div class="searchbar-container">
      <input
        type="text"
        class="search-input"
        placeholder="Search for music or playlists..."
        v-model="searchQuery"
        @input="handleSearch"
      />
      <button class="search-btn">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="search-icon">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l6 6m0 0l6-6m-6 6V10" />
        </svg>
      </button>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'SearchBar',
    data() {
      return {
        searchQuery: ''
      };
    },
    methods: {
      handleSearch() {
        console.log('Searching for:', this.searchQuery);
      }
    }
  });
  </script>
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
  
  .searchbar-container {
    display: flex;
    align-items: center;
    background-color: #0C0C0C;
    border: 2px solid #450920;
    border-radius: 30px;
    padding: 0.5rem 1rem;
    transition: border 0.3s ease, box-shadow 0.3s ease;
    width: 100%;
    max-width: 400px;
  }
  
  .searchbar-container:hover {
    border-color: #DA627D;
    box-shadow: 0px 4px 15px rgba(218, 98, 125, 0.5);
  }
  
  .search-input {
    background-color: transparent;
    border: none;
    color: #ffffff;
    outline: none;
    padding: 0.5rem;
    width: 100%;
    font-family: 'Poppins', sans-serif;
    font-size: 1rem;
  }
  
  .search-input::placeholder {
    color: #FFA5AB;
    opacity: 0.7;
    font-family: 'Poppins', sans-serif;
    transition: opacity 0.3s ease;
  }
  
  .search-input:focus::placeholder {
    opacity: 0.4;
  }
  
  .search-btn {
    background: none;
    border: none;
    cursor: pointer;
    margin-left: 0.5rem;
    transition: transform 0.3s ease;
  }
  
  .search-btn:hover {
    transform: scale(1.1);
  }
  
  .search-icon {
    width: 24px;
    height: 24px;
    stroke: #F9DF74;
    transition: stroke 0.3s ease;
  }
  
  .search-btn:hover .search-icon {
    stroke: #FFA5AB;
  }
  </style>
  