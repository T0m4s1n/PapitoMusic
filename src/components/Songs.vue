<template>
  <div class="songs-container">
    <h2 class="songs-title">Top Songs</h2>
    <div v-if="error" class="error-message">
      <p>Error fetching songs: {{ error }}</p>
    </div>

    <div v-if="songs.length > 0" class="song-list">
      <div v-for="(song, index) in songs" :key="index" class="song-item">
        <img :src="song.cover" alt="Thumbnail" class="album-cover" />
        <div class="song-info">
          <h3 class="song-name">{{ song.title }}</h3>
          <p class="song-channel">{{ song.artist }}</p>
        </div>
        <div class="button-group">
          <button @click="playSong(song)" class="play-button">▶</button>
          <button @click="addToPlaylist(song)" class="add-button">+</button>
        </div>
        <div v-if="index < songs.length - 1" class="divider"></div>
      </div>
    </div>

    <div class="playlist-container">
      <h3>Queue of songs (it works with doublelists jijiji :3)</h3>
      <div class="playlist-scroll">
        <div
          v-for="(song, index) in playlist.toArray()"
          :key="index"
          class="playlist-item"
          draggable="true"
          @dragstart="(event) => onDragStart(index, event)"
          @dragover.prevent
          @drop="onDrop(index)"
        >
          <img :src="song.cover" alt="Thumbnail" class="album-cover" />
          <div class="song-info">
            <h4>{{ song.title }}</h4>
            <p>{{ song.artist }}</p>
          </div>
          <button @click="removeFromPlaylist(index)" class="remove-button">
            ✖
          </button>
        </div>
      </div>
    </div>
    <div v-if="currentSong" class="mini-player slide-in">
      <p>Listening to: {{ currentSong.title }}</p>
      <img :src="currentSong.cover" alt="Cover" class="album-cover" />
      <button @click="playPreviousSong" class="prev-button"></button>
      <button @click="playNextSong" class="next-button"></button>
        <audio
          :src="currentSong.file"
          controls
          autoplay
          @ended="playNextSong"
        ></audio>
      </div>
      <button @click="closePlayer" class="close-player">✖</button>
    </div>
</template>
<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";

interface Song {
  title: string;
  artist: string;
  cover: string;
  file: string;
}

class Node<T> {
  value: T;
  next: Node<T> | null = null;
  prev: Node<T> | null = null;

  constructor(value: T) {
    this.value = value;
  }
}

class DoublyLinkedList<T> {
  head: Node<T> | null = null;
  tail: Node<T> | null = null;

  add(value: T) {
    const newNode = new Node(value);
    if (!this.tail) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      this.tail.next = newNode;
      newNode.prev = this.tail;
      this.tail = newNode;
    }
  }

  removeAt(index: number) {
    let current = this.head;
    let i = 0;
    while (current && i !== index) {
      current = current.next;
      i++;
    }
    if (!current) return;
    if (current.prev) current.prev.next = current.next;
    if (current.next) current.next.prev = current.prev;
    if (current === this.head) this.head = current.next;
    if (current === this.tail) this.tail = current.prev;
  }

  swap(index1: number, index2: number) {
    if (index1 === index2) return;

    let node1 = this.getNodeAt(index1);
    let node2 = this.getNodeAt(index2);

    if (node1 && node2) {
      let tempValue = node1.value;
      node1.value = node2.value;
      node2.value = tempValue;
    }
  }

  getNodeAt(index: number): Node<T> | null {
    let current = this.head;
    let i = 0;
    while (current && i !== index) {
      current = current.next;
      i++;
    }
    return current;
  }

  toArray(): T[] {
    const array: T[] = [];
    let current = this.head;
    while (current) {
      array.push(current.value);
      current = current.next;
    }
    return array;
  }
}

export default defineComponent({
  name: "SongsComponent",
  setup() {
    const songs = ref<Song[]>([]);
    const error = ref<string | null>(null);
    const currentSong = ref<Song | null>(null);
    const playlist = ref(new DoublyLinkedList<Song>());
    const dragIndex = ref<number | null>(null);

    const songList: Song[] = [
      {
        title: "505",
        artist: "Arctic Monkeys",
        cover: new URL("@/assets/music/505/505.jpg", import.meta.url).href,
        file: new URL("@/assets/music/505/505.mp4", import.meta.url).href,
      },
      {
        title: "Why'd You Only Call Me When You're High?",
        artist: "Arctic Monkeys",
        cover: new URL("@/assets/music/Whyd/am.jpg", import.meta.url).href,
        file: new URL("@/assets/music/Whyd/why.mp4", import.meta.url).href,
      },
      {
        title: "Good Looking",
        artist: "Suki Waterhouse",
        cover: new URL("@/assets/music/Good/good.jpg", import.meta.url).href,
        file: new URL("@/assets/music/Good/good.mp4", import.meta.url).href,
      },
      {
        title: "Blinding lights",
        artist: "The Weeknd",
        cover: new URL("@/assets/music/blinding/blinding.jpg", import.meta.url)
          .href,
        file: new URL(
          "@/assets/music/blinding/blindinglights.mp4",
          import.meta.url
        ).href,
      },
      {
        title: "Lovers is a Day",
        artist: "Cuco",
        cover: new URL("@/assets/music/cuco/cuco.jpg", import.meta.url).href,
        file: new URL("@/assets/music/cuco/cuco.mp4", import.meta.url).href,
      },
      {
        title: "DARE",
        artist: "Gorillaz",
        cover: new URL("@/assets/music/dare2/dare2.jpg", import.meta.url).href,
        file: new URL("@/assets/music/dare2/dare.mp4", import.meta.url).href,
      },
      {
        title: "Gasoline",
        artist: "The Weeknd",
        cover: new URL("@/assets/music/gasoline/gasoline.jpg", import.meta.url)
          .href,
        file: new URL("@/assets/music/gasoline/gasoline.mp4", import.meta.url)
          .href,
      },
      {
        title: "Lovers Rock",
        artist: "Tv Girl",
        cover: new URL("@/assets/music/loversRock/lovers.jpg", import.meta.url)
          .href,
        file: new URL("@/assets/music/loversRock/lovers.mp4", import.meta.url)
          .href,
      },
      {
        title: "Redbone",
        artist: "Childish Gambino",
        cover: new URL("@/assets/music/redbone/redbone.jpg", import.meta.url)
          .href,
        file: new URL("@/assets/music/redbone/redbone.mp4", import.meta.url)
          .href,
      },
      {
        title: "Paradise",
        artist: "WillyRex",
        cover: new URL("@/assets/music/willyrex/willy.jpg", import.meta.url)
          .href,
        file: new URL("@/assets/music/willyrex/willy.mp4", import.meta.url)
          .href,
      },
      {
        title: "I Really Want To Stay At Your House",
        artist: "Rosa Walton",
        cover: new URL("@/assets/music/cyber/cyber.jpg", import.meta.url).href,
        file: new URL("@/assets/music/cyber/cyber.mp4", import.meta.url).href,
      },
      {
        title: "Buenos dias Amor [LIVE]",
        artist: "Jose Jose",
        cover: new URL(
          "@/assets/music/buenosdiasamor/buenosdiasamor.jpg",
          import.meta.url
        ).href,
        file: new URL(
          "@/assets/music/buenosdiasamor/buenosdiasamor.mp4",
          import.meta.url
        ).href,
      },
      {
        title: "Gotas de fuego",
        artist: "Jose Jose",
        cover: new URL(
          "@/assets/music/gotasfuego/gotasfuego.jpg",
          import.meta.url
        ).href,
        file: new URL(
          "@/assets/music/gotasfuego/gotasfuego.mp4",
          import.meta.url
        ).href,
      },
    ];

    const getPlaylistSongs = () => {
      try {
        songs.value = songList;
      } catch (err) {
        error.value = "Failed to load songs.";
        console.error(err);
      }
    };

    const playSong = (song: Song) => {
      currentSong.value = song;
    };

    const addToPlaylist = (song: Song) => {
      playlist.value.add(song);

      if (!currentSong.value) {
        currentSong.value = playlist.value.head?.value || null;
      }
    };

    const removeFromPlaylist = (index: number) => {
      playlist.value.removeAt(index);
    };

    const playNextSong = () => {
      if (!playlist.value.head) {
        currentSong.value = null;
        return;
      }
      let currentNode = playlist.value.head;
      if (!currentSong.value) {
        currentSong.value = playlist.value.head?.value || null;
        return;
      }
      while (currentNode && currentNode.value !== currentSong.value) {
        if (currentNode.next) {
          currentNode = currentNode.next;
        } else {
          break;
        }
      }
      if (currentNode?.next) {
        currentSong.value = currentNode.next.value;
      } else {
        currentSong.value = playlist.value.head?.value || null;
      }
      if (playlist.value.head) {
        playlist.value.removeAt(0);
      }
    };

    const playPreviousSong = () => {
      if (!playlist.value.head) {
        currentSong.value = null;
        return;
      }
      let currentNode = playlist.value.tail;
      if (!currentSong.value) {
        currentSong.value = playlist.value.tail?.value || null;
        return;
      }
      while (currentNode && currentNode.value !== currentSong.value) {
        if (currentNode.prev) {
          currentNode = currentNode.prev;
        } else {
          break;
        }
      }
      if (currentNode?.prev) {
        currentSong.value = currentNode.prev.value;
      } else {
        currentSong.value = playlist.value.tail?.value || null;
      }
    };

    const closePlayer = () => {
      currentSong.value = null;
    };

    const onDragStart = (index: number, event: DragEvent) => {
      dragIndex.value = index;
      const target = event.target as HTMLElement;
      target.classList.add("dragging");
      event.dataTransfer?.setDragImage(new Image(), 0, 0);
    };

    const onDragEnd = (event: DragEvent) => {
      const target = event.target as HTMLElement;
      target.classList.remove("dragging");
      dragIndex.value = null;
    };

    const onDrop = (index: number) => {
      if (dragIndex.value !== null && dragIndex.value !== index) {
        playlist.value.swap(dragIndex.value, index);
      }
      dragIndex.value = null;
    };

    onMounted(() => {
      getPlaylistSongs();
      if (playlist.value.head) {
        currentSong.value = playlist.value.head.value;
      }
    });

    return {
      songs,
      playSong,
      addToPlaylist,
      playlist,
      removeFromPlaylist,
      error,
      currentSong,
      closePlayer,
      playNextSong,
      onDragStart,
      onDrop,
      playPreviousSong,
    };
  },
});
</script>
<style scoped>
.songs-container {
  padding: 20px;
}

.songs-title {
  color: #ffffff;
  font-family: "Poppins", sans-serif;
  font-size: 24px;
  margin-bottom: 20px;
  text-align: center;
}

.song-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.song-item {
  background-color: #0c0c0c;
  border-radius: 8px;
  padding: 15px;
  color: white;
  width: 250px;
  height: 350px;
  text-align: center;
  transition: transform 0.2s ease-in-out;
  position: relative;
  user-select: none;
}

.song-item:hover {
  transform: scale(1.05);
}

.album-cover {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 8px;
  margin-left: 19px;
}

.song-info {
  margin-top: 10px;
}

.song-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.song-channel {
  font-size: 14px;
  color: #ffa5ab;
}

.button-group {
  display: flex;
  justify-content: space-around;
  margin-top: 10px;
}

.play-button,
.add-button {
  background-color: #da627d;
  color: white;
  padding: 8px 12px;
  text-decoration: none;
  border-radius: 50%;
  display: inline-block;
  transition: background-color 0.3s ease;
  cursor: pointer;
}

.play-button:hover,
.add-button:hover {
  background-color: #f9df74;
}

.divider {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 1px;
  background-color: rgba(255, 255, 255, 0.2);
  margin-top: 15px;
}

.mini-player {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 320px;
  height: 200px;
  background-color: #0c0c0c;
  border-radius: 8px;
  z-index: 10;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0px 4px 10px rgba(218, 98, 125, 0.5);
  padding: 10px;
  animation: slide-in 0.5s ease-in-out;
  font-family: "Poppins", sans-serif;
}

@keyframes slide-in {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.mini-player audio {
  width: 100%;
  height: 40px;
  background-color: transparent;
  color: #ffffff;
  border-radius: 5px;
  margin-top: 5px;
  appearance: none;
  font-family: "Geist", Monospace;
}

.prev-button,
.next-button {
  background-color: #da627d;
  color: white;
  padding: 4px 8px;
  text-decoration: none;
  border-radius: 50%;
  display: inline-block;
  transition: background-color 0.3s ease;
  cursor: pointer;
  font-size: 12px;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

.prev-button:hover,
.next-button:hover {
  background-color: #f9df74;
  transition: background-color 0.3s ease;
}

.prev-button {
  left: 10px;
}

.next-button {
  right: 10px;
}

.prev-button::before {
  content: "◀";
}

.next-button::before {
  content: "▶";
}

.mini-player .album-cover {
  width: 70px;
  height: 50px;
  object-fit: cover;
  border-radius: 5px;
  margin-bottom: 10px;
}

.mini-player audio::-webkit-media-controls-panel {
  background-color: #da627d;
  color: #ffffff;
  font-family: "Poppins", sans-serif;
}

.mini-player audio::-webkit-media-controls-play-button,
.mini-player audio::-webkit-media-controls-pause-button {
  background-color: #da627d;
  border-radius: 50%;
  padding: 5px;
}

.mini-player audio::-webkit-media-controls-timeline {
  background-color: #da627d;
  border-radius: 5px;
}

.mini-player audio::-webkit-media-controls-current-time-display,
.mini-player audio::-webkit-media-controls-time-remaining-display {
  color: #ffffff;
}

.mini-player audio::-webkit-media-controls-volume-slider-container,
.mini-player audio::-webkit-media-controls-mute-button {
  background-color: #da627d;
}

.mini-player audio::-moz-progress-bar {
  background-color: #da627d;
}

.mini-player audio::--moz-time-text {
  color: #ffffff;
}

.mini-player audio::-moz-mute-button {
  background-color: #da627d;
}

.close-player {
  position: absolute;
  top: 5px;
  right: 5px;
  background-color: transparent;
  border: none;
  color: #ffffff;
  font-size: 16px;
  cursor: pointer;
}

.close-player:hover {
  color: #ffa5ab;
}

.playlist-container {
  padding: 10px;
  background-color: #0c0c0c;
  margin-top: 100px;
  border-radius: 8px;
  max-height: 400px;
  overflow-y: auto;
  width: 500px;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  scrollbar-color: #da627d #1c1c1c;
  scrollbar-width: thin;
}

.playlist-container::-webkit-scrollbar {
  width: 8px;
}

.playlist-container::-webkit-scrollbar-track {
  background: #1c1c1c;
  border-radius: 10px;
}

.playlist-container::-webkit-scrollbar-thumb {
  background: #da627d;
  border-radius: 10px;
}

.playlist-container::-webkit-scrollbar-thumb:hover {
  background: #f9df74;
}

.playlist-scroll {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.playlist-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #1c1c1c;
  padding: 10px;
  border-radius: 5px;
}

.playlist-item img {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 5px;
}

.remove-button {
  background-color: #da627d;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 9px;
}

.remove-button:hover {
  background-color: #f9df74;
}
</style>
