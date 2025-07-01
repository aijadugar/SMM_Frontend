<template>
  <div class="hashtag-generator">
    <h1 class="heading">Generate Hashtags for Your Blog/Post</h1>

    <div class="input-box">
      <textarea
        v-model="topic"
        placeholder="Enter your blog/post topic here..."
        rows="4"
      ></textarea>
      <button @click="submit" :disabled="loading">
        {{ loading ? "Generating..." : "Generate Hashtags" }}
      </button>
    </div>

    <div v-if="loading" class="loading-box">
      <div class="loader"></div>
      <p>Generating hashtags, please wait...</p>
    </div>

    <div v-if="hashtags.length && !loading" class="result-section">
      <h3>📢 Suggested Hashtags:</h3>
      <textarea
        readonly
        :value="hashtags.join(' ')"
        class="hashtags-textarea"
        rows="4"
        style="
          font-family: 'Times New Roman', serif;
          background: #f8f4e5;
          border: 2px solid #2c3e50;
          border-radius: 4px;
          box-shadow: inset 2px 2px 4px rgba(0,0,0,0.1);
          padding: 15px;
          font-size: 16px;
          line-height: 1.6;
          color: #2c3e50;
          width: 100%;
        "
      ></textarea>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

const topic = ref<string>('')
const hashtags = ref<string[]>([])
const loading = ref<boolean>(false)
const BASE_URL = import.meta.env.DEV
  ? 'http://127.0.0.1:5000'
  : 'https://smm-backend-wljg.onrender.com'

async function submit() {
  if (!topic.value) {
    alert('Please enter a topic or blog description.')
    return
  }

  loading.value = true
  hashtags.value = []

  try {
    const response = await axios.post(`${BASE_URL}/hashtags`, {
      topic: topic.value,
    })
    hashtags.value = response.data.hashtags
  } catch (err) {
    console.error('Error:', err)
    hashtags.value = ['Something went wrong!']
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.hashtag-generator {
  max-width: 700px;
  margin: 2rem auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  padding: 2rem;
  background-color: #f9f9f9;
  border-radius: 15px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
}

.hashtags-textarea {
  font-family: 'Times New Roman', serif;
  background: #f8f4e5;
  border: 2px solid #2c3e50;
  border-radius: 4px;
  box-shadow: inset 2px 2px 4px rgba(0,0,0,0.1);
  padding: 15px;
  font-size: 16px;
  line-height: 1.6;
  color: #2c3e50;
  width: 100%;
  resize: none;
}

.heading {
  font-size: 2rem;
  color: #2c3e50;
  margin-bottom: 1.5rem;
}

.input-box {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

textarea {
  padding: 0.8rem;
  border-radius: 8px;
  font-size: 1rem;
  border: 1px solid #ccc;
  resize: none;
}

button {
  padding: 0.7rem 2rem;
  background-color: #2c3e50;
  color: white;
  border: none;
  font-size: 1rem;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover:not(:disabled) {
  background-color: #1a252f;
}

button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

.loading-box {
  margin-top: 2rem;
  text-align: center;
}

.loader {
  border: 6px solid #f3f3f3;
  border-top: 6px solid #2c3e50;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  margin: 0 auto 1rem auto;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.result-section {
  margin-top: 2rem;
  text-align: left;
  padding: 1rem;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.result-section h3 {
  color: #2c3e50;
  margin-bottom: 1rem;
}

.result-section ul {
  list-style: none;
  padding: 0;
}

.result-section li {
  background-color: #e8f0fe;
  color: #2c3e50;
  margin-bottom: 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-weight: 500;
}
</style>
