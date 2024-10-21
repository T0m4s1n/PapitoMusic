<template>
  <section class="hero">
    <div id="threejs-background"></div>
    <div class="hero-content">
      <h1 class="hero-title">Discover the Best Music</h1>
      <p class="hero-subtitle">Your journey to the finest tracks starts here</p>
    </div>
  </section>
</template>

<script lang="ts">
import { onMounted } from "vue";
import * as THREE from "three";

export default {
  name: "HeroComponent",
  setup() {
    const initThreeJS = () => {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      const renderer = new THREE.WebGLRenderer({ alpha: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
      document
        .getElementById("threejs-background")
        ?.appendChild(renderer.domElement);

      const particleGeometry = new THREE.BufferGeometry();
      const particleCount = 2000;
      const positions = new Float32Array(particleCount * 3);
      const colors = [];

      for (let i = 0; i < particleCount * 3; i += 3) {
        positions[i] = (Math.random() - 0.5) * 10;
        positions[i + 1] = (Math.random() - 0.5) * 10;
        positions[i + 2] = (Math.random() - 0.5) * 10;

        const color = new THREE.Color();
        color.set(Math.random() < 0.5 ? "#FFA5AB" : "#DA627D");
        colors.push(color.r, color.g, color.b);
      }

      particleGeometry.setAttribute(
        "position",
        new THREE.BufferAttribute(positions, 3)
      );
      particleGeometry.setAttribute(
        "color",
        new THREE.BufferAttribute(new Float32Array(colors), 3)
      );

      const particleMaterial = new THREE.PointsMaterial({
        size: 0.02,
        vertexColors: true,
        transparent: true,
        opacity: 1,
        blending: THREE.AdditiveBlending,
        depthWrite: false,
      });

      const particles = new THREE.Points(particleGeometry, particleMaterial);

      scene.add(particles);
      camera.position.z = 5;

      const animate = () => {
        requestAnimationFrame(animate);
        particles.rotation.y += 0.001;
        renderer.render(scene, camera);
      };

      animate();
    };

    onMounted(() => {
      initThreeJS();
    });
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");

.hero {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(180deg, #0c0c0c 0%, #450920 100%);
  position: relative;
  overflow: hidden;
}

#threejs-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.hero-content {
  z-index: 2;
  text-align: center;
  color: white;
  position: relative;
  animation: fadeInUp 1.5s ease forwards;
}

.hero-title {
  font-family: "Poppins", sans-serif;
  font-size: 3rem;
  font-weight: 700;
  color: #f9df74;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 1.5s ease forwards,
    moveOnHover 1s ease-in-out infinite alternate;
}

.hero-subtitle {
  font-family: "Poppins", sans-serif;
  font-size: 1.5rem;
  font-weight: 400;
  margin-top: 1rem;
  color: #ffa5ab;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 2s ease forwards 0.3s;
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes moveOnHover {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-10px);
  }
}

.hero-title:hover,
.hero-subtitle:hover {
  animation: hoverEffect 0.5s ease forwards;
}

@keyframes hoverEffect {
  to {
    transform: scale(1.05);
  }
}
</style>
