<template>
  <section class="faq-hero">
    <div id="threejs-model"></div>
    <div class="faq-content">
      <h1 class="faq-title">Frequently Asked Questions</h1>
      <div v-for="(faq, index) in faqs" :key="index" class="faq-item">
        <button class="faq-question" @click="toggleFaq(index)">
          {{ faq.question }}
          <span v-if="activeIndex === index">−</span>
          <span v-else>+</span>
        </button>
        <div v-if="activeIndex === index" class="faq-answer">
          <p>{{ faq.answer }}</p>
        </div>
      </div>
    </div>
  </section>
  <br />
  <br />
</template>
<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";
import * as THREE from "three";
import texture1 from "../assets/cubocaratula/505.jpg";
import texture2 from "../assets/cubocaratula/cuco.jpg";
import texture3 from "../assets/cubocaratula/good.jpg";
import texture4 from "../assets/cubocaratula/am.jpg";
import texture5 from "../assets/cubocaratula/dare.jpg";
import texture6 from "../assets/cubocaratula/dare2.jpg";

export default defineComponent({
  name: "FAQHero",
  setup() {
    const faqs = ref([
      {
        question: "What is this platform about?",
        answer:
          "PapitoMusic allows you to create and manage music playlists, play songs, and explore top hits.",
      },
      {
        question: "How do I add songs to my playlist?",
        answer:
          'Click the "+" button next to the song in the list to add it to your playlist.',
      },
      {
        question: "Can I play songs offline?",
        answer:
          "No, songs can only be streamed while connected to the internet.",
      },
      {
        question: "What file formats are supported?",
        answer: "We support MP3, MP4, and other popular formats for playback.",
      },
      {
        question: "Is there a mobile app?",
        answer: "No, the platform is only available as a web and desktop app.",
      },
    ]);

    const activeIndex = ref<number | null>(null);

    const toggleFaq = (index: number): void => {
      activeIndex.value = activeIndex.value === index ? null : index;
    };

    const cubeTextures = [
      texture1,
      texture2,
      texture3,
      texture4,
      texture5,
      texture6,
    ];

    const initThreeCube = () => {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        50,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      const renderer = new THREE.WebGLRenderer({ alpha: true });
      renderer.setSize(window.innerWidth / 2, window.innerHeight / 2);
      document
        .getElementById("threejs-model")
        ?.appendChild(renderer.domElement);

      const loader = new THREE.TextureLoader();
      const materials = cubeTextures.map((texture) => {
        return new THREE.MeshBasicMaterial({
          map: loader.load(
            texture,
            (texture) => {
              console.log(`Loaded texture: ${texture.image.src}`);
            },
            undefined,
            (error) => {
              console.error(`Error loading texture: ${texture}`, error);
            }
          ),
          side: THREE.DoubleSide,
        });
      });

      const geometry = new THREE.BoxGeometry();
      const cube = new THREE.Mesh(geometry, materials);
      scene.add(cube);

      camera.position.z = 3;

      const animate = () => {
        requestAnimationFrame(animate);
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;
        renderer.render(scene, camera);
      };

      animate();
    };

    onMounted(() => {
      initThreeCube();
    });

    return {
      faqs,
      activeIndex,
      toggleFaq,
    };
  },
});
</script>
<style>
.faq-hero {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 40px;
  height: 80vh;
  background: linear-gradient(180deg, #0c0c0c 0%, #450920 100%);
}

#threejs-model {
  width: 45%;
  height: 80vh;
  background: transparent;
  position: relative;
}

.faq-content {
  width: 45%;
  z-index: 2;
  text-align: left;
  color: white;
  animation: fadeInUp 1s ease forwards;
  display: flex;
  flex-direction: column;
  align-items: stretch;
}

.faq-title {
  font-family: "Poppins", sans-serif;
  font-size: 2.5rem;
  color: #f9df74;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 1s ease forwards;
}

.faq-item {
  margin: 15px 0;
}

.faq-question {
  background-color: #0c0c0c;
  color: white;
  font-family: "Poppins", sans-serif;
  font-size: 1.25rem;
  padding: 10px 15px;
  border: none;
  text-align: left;
  cursor: pointer;
  transition: background-color 0.3s ease;
  box-shadow: 0px 4px 15px rgba(218, 98, 125, 0.5);
  border-radius: 4px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.faq-question:hover {
  background-color: #da627d;
}

.faq-answer {
  background-color: #121212;
  color: white;
  margin-top: 10px;
  padding: 10px;
  border-left: 4px solid #da627d;
  border-radius: 4px;
  animation: slide-down 0.3s ease-in-out;
}

@keyframes slide-down {
  from {
    max-height: 0;
    opacity: 0;
  }
  to {
    max-height: 200px;
    opacity: 1;
  }
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
