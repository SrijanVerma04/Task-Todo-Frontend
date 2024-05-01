<template>
  <div id="app" class="container mx-auto px-4 py-8 bg-gray-100">
    <header class="flex justify-between items-center py-4 px-6 bg-white shadow-md rounded-lg">
      <h1 class="text-5xl font-bold text-orange-700">Todo-List</h1>
      <nav class="space-x-4 font-semibold">
        <a href="/" class="text-gray-700 hover:text-green-500">Home</a>
        <a v-if="!isLoggedIn" href="/auth/login" class="text-gray-700 hover:text-green-500">Login</a>
        <a v-if="isLoggedIn" @click="handleLogout" class="text-gray-700 hover:text-green-500">Logout</a>
      </nav>
    </header>

    <div v-for="todo in todos" :key="todo.id" class="mb-6 bg-white rounded-lg shadow-md px-6 py-4 flex flex-col space-y-4">
      </div>

  </div>
</template>

<script setup>

import { ref, onMounted } from 'vue';
import axios from 'axios';
import {url} from '../url.js'

const userData = ref(null);
const isLoggedIn = ref(false);

const getUser = async() => {
    try {
      const response = await axios.get(`${url}/api/auth/refresh`, { withCredentials: true });
      console.log("response",response.data);
      userData.value = response.data;
      // console.log('User data:', userData.value);
      if(userData.value){
        isLoggedIn.value = true;
      }
    }
    catch (error) {
      console.error('Error fetching user data:', error);
    }
  }

  onMounted(getUser);

  const handleLogout = async() => {
    try {
      const res = await axios.get(`${url}/api/auth/logout`, { withCredentials: true });
      console.log("res",res);
      userData.value = null;
      isLoggedIn.value = false;
      window.location.href = '/';
    } catch (error) {
      console.error('Error logging out:', error);
    }
  }

</script>

<style scoped>
  /* No scoped styles needed for this example */
</style>

