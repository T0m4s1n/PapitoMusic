<template>
  <footer class="footer">
    <div class="footer-content">
      <div class="footer-section about">
        <h2 class="footer-title">PapitoMusic</h2>
        <p class="footer-text">
          Your platform to discover, listen, and share music with the world.
        </p>
        <div class="social-icons">
          <a href="#" class="social-link"><i class="fab fa-facebook"></i></a>
          <a href="#" class="social-link"><i class="fab fa-twitter"></i></a>
          <a href="#" class="social-link"><i class="fab fa-instagram"></i></a>
        </div>
      </div>

      <div class="footer-section links">
        <h2 class="footer-title">Useful Links</h2>
        <ul class="footer-list">
          <li><a href="#" class="footer-link">Home</a></li>
          <li><a href="#" class="footer-link">Discover</a></li>
          <li><a href="#" class="footer-link">Playlists</a></li>
          <li><a href="#" class="footer-link">Profile</a></li>
        </ul>
      </div>

      <div class="footer-section subscribe">
        <h2 class="footer-title">Subscribe</h2>
        <p class="footer-text">
          Get updates on the latest playlists and music releases.
        </p>
        <form class="subscribe-form">
          <input
            type="email"
            placeholder="Email Address"
            class="subscribe-input"
          />
          <button class="subscribe-button">Subscribe</button>
        </form>
      </div>
    </div>
    <div class="footer-bottom">
      <p>&copy; 2024 PapitoMusic. All rights reserved.</p>
      <p>Designed by T0m4s1n</p>
    </div>
  </footer>
</template>
<script lang="ts">
export default {
  name: "FooterComponent",
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap");

.footer {
  background-color: #0c0c0c;
  padding: 2rem 1rem;
  bottom: 0;
  color: white;
  font-family: "Poppins", sans-serif;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  flex-wrap: wrap;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.footer-section {
  margin-bottom: 1.5rem;
  width: 30%;
}

.footer-title {
  color: #f9df74;
  font-weight: 600;
  font-size: 1.2rem;
  margin-bottom: 1rem;
}

.footer-text {
  color: #ffffff;
  font-size: 0.9rem;
  margin-bottom: 1rem;
}

.footer-list {
  list-style: none;
  padding: 0;
}

.footer-list li {
  margin-bottom: 0.5rem;
}

.footer-link {
  color: #ffffff;
  text-decoration: none;
  font-size: 1rem;
  transition: color 0.3s ease;
}

.footer-link:hover {
  color: #ffa5ab;
}

.social-icons {
  display: flex;
  justify-content: flex-start;
  gap: 0.5rem;
}

.social-link {
  color: #f9df74;
  font-size: 1.2rem;
  transition: color 0.3s ease, transform 0.3s ease;
}

.social-link:hover {
  color: #ffa5ab;
  transform: scale(1.1);
}

.subscribe-form {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  font-family: "Poppins", sans-serif;
}

.subscribe-input {
  padding: 0.5rem;
  border: none;
  border-radius: 5px;
  outline: none;
  background-color: #1a1a1a;
  color: white;
  font-family: "Poppins", sans-serif;
}

.subscribe-button {
  padding: 0.5rem 1rem;
  background-color: #da627d;
  color: white;
  border: none;
  border-radius: 5px;
  margin-left: 0.5rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-family: "Poppins", sans-serif;
}

.subscribe-button:hover {
  background-color: #ffa5ab;
}

.footer-bottom {
  text-align: center;
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #450920;
}

.footer-bottom p {
  color: #ffffff;
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .footer-content {
    flex-direction: column;
    align-items: center;
  }

  .footer-section {
    width: 100%;
    text-align: center;
  }
}
</style>
