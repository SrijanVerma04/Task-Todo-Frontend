<template>
    <div class="min-h-screen bg-gray-100 flex justify-center items-center">
      <div class="w-full max-w-md bg-white rounded-lg shadow-md px-8 py-10 flex flex-col space-y-6">
        <h1 class="text-3xl font-bold text-center text-gray-800"> Signup </h1>
  
        <div v-if="error" class="text-red-500 text-sm text-center">Something Went Wrong</div>

        <div class="flex flex-col space-y-2">
          <label for="username" class="text-sm font-medium text-gray-700">UserName</label>
          <input
            type="text"
            id="text"
            v-model="username"
            placeholder="Enter your Name"
            class="w-full px-3 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-1 focus:ring-blue-500"
          />
        </div>

        <div class="flex flex-col space-y-2">
          <label for="email" class="text-sm font-medium text-gray-700">Email address</label>
          <input
            type="email"
            id="email"
            v-model="email"
            placeholder="Enter your email"
            class="w-full px-3 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-1 focus:ring-blue-500"
          />
        </div>
  
        <div class="flex flex-col space-y-2">
          <label for="password" class="text-sm font-medium text-gray-700">Password</label>
          <input
            type="password"
            id="password"
            v-model="password"
            placeholder="Enter your password"
            class="w-full px-3 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-1 focus:ring-blue-500"
          />
        </div>
  
        <button @click="handleSubmit" class="w-full py-2 rounded-lg bg-blue-500 text-white font-medium hover:bg-blue-700">
          SignUp
        </button>
  
        <!-- <div class="w-full flex justify-start items-center space-x-2">
          <p class="italic">Already Registered?</p>
          <router-link :to="{ path: '/login' }" class="font-semibold px-4 py-[4px] bg-slate-400 hover:bg-fuchsia-300 rounded-lg">Login</router-link>
        </div>       -->
  
        <div class="flex items-center justify-between mt-4">
          <p class="text-sm text-gray-500">
            If Registered then login : 
            <router-link :to="{ path: '/login' }" class="text-blue-500 underline hover:text-blue-700"> Login </router-link>
          </p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import {url} from '../url.js'
  
  export default {
    data() {
      return {
        username: '',
        email: '',
        password: '',
        error: false,
      };
    },
    methods: {
      isEmailValid(email) {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(email);
      },

      async handleSubmit() {
        if (!this.username || !this.email || !this.password) {
          this.error = true;
          return;
        }
        if (!this.isEmailValid(this.email)) {
          this.error = true;
          return;
        }
  
        try {
          const API_AUTH = `${url}/api/auth`;
          const res = await axios.post(`${API_AUTH}/signup`, {
            username: this.username,
            email: this.email,
            password: this.password,
          });
  
          console.log('res->', res);
          this.resetForm();
          window.location.href = '/auth/login';
        } 
        catch (err) {
          this.error = true;
          console.error(err);
        }
      },
      resetForm() {
        this.username = '';
        this.email = '';
        this.password = '';
        this.error = false;
      },
    },
  };
  </script>
  