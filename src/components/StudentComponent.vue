<template>
    <div class="lookup-container">
      <h2>Student Lookup</h2>
      <input v-model="rollNumber" placeholder="Enter Roll Number" />
      <button @click="fetchStudent">Search</button>
  
      <div v-if="student">
        <p><strong>Roll:</strong> {{ student.roll }}</p>
        <p><strong>Name:</strong> {{ student.name }}</p>
      </div>
  
      <div v-if="error" class="error">{{ error }}</div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import axios from 'axios';
  
  const rollNumber = ref('');
  const student = ref(null);
  const error = ref('');
  
  const fetchStudent = async () => {
    student.value = null;
    error.value = '';
  
    if (!rollNumber.value) {
      error.value = 'Please enter a roll number.';
      return;
    }
  
    try {
      const response = await axios.get(`http://127.0.0.1:8000/api/student/${rollNumber.value}/`);
      student.value = response.data;
    } catch (err) {
      error.value = err.response?.data?.error || 'Student not found';
    }
  };
  </script>
  
  <style scoped>
  .lookup-container {
    max-width: 400px;
    margin: auto;
    padding: 20px;
    text-align: center;
  }
  input {
    padding: 8px;
    width: 200px;
    margin-right: 10px;
  }
  button {
    padding: 8px 16px;
  }
  .error {
    color: red;
    margin-top: 10px;
  }
  </style>
  