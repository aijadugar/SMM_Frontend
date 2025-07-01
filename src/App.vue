<script setup lang="ts">
import { ref } from 'vue'
import TextToImageComponent from './components/TextToImageComponent.vue'
import ImageMockupComponent from './components/ImageMockupComponent.vue'

const activeComponent = ref('')
</script>

<template>
  <div class="app-container">
    <!-- Logo/Header (Top-Left) -->
    <header class="header">
      <h1 class="logo">🧠 The SMM Hub Solutions</h1>
    </header>

    <!-- Feature Buttons -->
    <div class="button-section">
      <button @click="activeComponent = 'textToImage'">🖼️ Text to Image</button>
      <button @click="activeComponent = 'imageMockup'">🧩 Keywords / Hashtags</button>
    </div>

    <!-- Dynamic Component Area -->
    <div class="feature-area">
      <TextToImageComponent v-if="activeComponent === 'textToImage'" />
      <ImageMockupComponent v-if="activeComponent === 'imageMockup'" />
    </div>
  </div>
</template>

<style scoped>
/* Full screen layout */
html,
body,
.app-container {
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100vw;
  background-color: #f5f7fa;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  overflow: hidden;
}

/* Header top-left */
.header {
  position: absolute;
  top: 1rem;
  left: 1.5rem;
}

.logo {
  font-size: 1.8rem;
  color: #2c3e50;
  text-shadow: 1px 1px 2px #ccc;
  margin: 0;
}

/* Center button section */
.button-section {
  position: absolute;
  top: 6rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 1rem;
}

.button-section button {
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.button-section button:hover {
  background-color: #1a252f;
}

/* Component display area */
.feature-area {
  position: absolute;
  top: 10rem;
  left: 50%;
  transform: translateX(-50%);
  width: 90%;
  max-width: 900px;
  overflow-y: auto;
  height: auto;
  padding-bottom: 2rem;
}
</style>
