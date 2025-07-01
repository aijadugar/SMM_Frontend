<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

const userInput = ref('')
const generatedLink = ref('')
const loading = ref(false)

const BASE_URL = import.meta.env.DEV
  ? 'http://127.0.0.1:5000'
  : 'https://smm-backend-wljg.onrender.com'

const submitText = async () => {
  loading.value = true
  generatedLink.value = ''
  try {
    const response = await axios.post(`${BASE_URL}/generate-link`, {
      text: userInput.value
    })
    generatedLink.value = response.data.link
  } catch (error) {
    console.error('Error:', error)
    generatedLink.value = 'Something went wrong...!'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="container">
    <h1 class="main-heading">Generate Image Link from Text</h1>

    <div class="form-box">
      <textarea v-model="userInput" placeholder="Type your text here..." rows="5"></textarea>
      <button @click="submitText" :disabled="loading">
        {{ loading ? "Generating..." : "Generate Image Link" }}
      </button>

      <div v-if="loading" class="loader-box">
        <div class="loader"></div>
        <p class="loading-text">Please wait, generating your link...</p>
      </div>

      <div v-if="generatedLink && !loading" class="result-box">
        <p><strong>📢 Your Link:</strong></p>
        <a :href="generatedLink" target="_blank" class="link">{{ generatedLink }}</a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 700px;
  margin: 0 auto;
  padding: 2rem 1rem;
  font-family: 'Segoe UI', sans-serif;
  text-align: center;
  box-sizing: border-box;
  flex-direction: column;
  justify-content: center;
  overflow-y: auto;
}

.main-heading {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 2rem;
  font-weight: 600;
  word-break: break-word;
}

.form-box {
  background-color: #ffffff;
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease-in-out;
}

textarea {
  width: 100%;
  padding: 1rem;
  font-size: 1rem;
  border-radius: 10px;
  border: 1px solid #ccc;
  resize: none;
  margin-bottom: 1.2rem;
  box-sizing: border-box;
  transition: border 0.3s ease;
}

textarea:focus {
  border-color: #2c3e50;
  outline: none;
}

button {
  background-color: #2c3e50;
  color: white;
  border: none;
  padding: 0.8rem 2rem;
  font-size: 1rem;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-top: 0.5rem;
}

button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  background-color: #1a252f;
}

.loader-box {
  margin-top: 1.8rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.loader {
  border: 5px solid #f3f3f3;
  border-top: 5px solid #2c3e50;
  border-radius: 50%;
  width: 45px;
  height: 45px;
  animation: spin 1s linear infinite;
  margin-bottom: 0.8rem;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.loading-text {
  font-size: 1rem;
  color: #2c3e50;
}

.result-box {
  margin-top: 1.8rem;
  padding: 1rem 1.5rem;
  background-color: #e8f0fe;
  border-left: 5px solid #2c3e50;
  border-radius: 8px;
  text-align: left;
  word-break: break-word;
}

.result-box p {
  margin: 0;
  font-weight: 500;
}

.link {
  display: inline-block;
  margin-top: 0.6rem;
  color: #1a252f;
  font-weight: 600;
  text-decoration: underline;
  transition: color 0.2s ease;
}

.link:hover {
  color: #0f1419;
}

/* Responsive styles */
@media (max-width: 600px) {
  .main-heading {
    font-size: 1.8rem;
  }

  .form-box {
    padding: 1.5rem;
  }

  button {
    width: 100%;
  }
}
</style>
