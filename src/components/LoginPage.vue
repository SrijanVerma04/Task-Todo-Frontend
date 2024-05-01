  <template>
    <div class="min-h-screen bg-gray-100 flex justify-center items-center">
      <div class="w-full max-w-md bg-white rounded-lg shadow-md px-8 py-10 flex flex-col space-y-6">
        <h1 class="text-3xl font-bold text-center text-gray-800">Login</h1>
  
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
  
        <button @click="handleLogin" class="w-full py-2 rounded-lg bg-blue-500 text-white font-medium hover:bg-blue-700">
          Login
        </button>
  
        <div v-if="error" class="text-red-500 text-sm text-center">Something Went Wrong</div>
  
        <div class="flex items-center justify-between mt-4">
          <p class="text-sm text-gray-500">
            Not Registered?
            <a href="/auth/signup" class="text-blue-500 underline hover:text-blue-700"> Sign Up </a>
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
        email: "",
        password: "",
        error: false,
      };
    },
    methods: {
      async handleLogin() {
        if (!this.email || !this.password) {
          this.error = true;
          return;
        }
  
        try {
          const API_AUTH = `${url}/api/auth`;
          const response = await axios.post(`${API_AUTH}/login`, {
            email: this.email,
            password: this.password,
          },{
            withCredentials: true,
          });
  
          console.log('Login response:', response);
          this.resetForm();
          window.location.href = '/';
        } 
        catch (err) {
          console.error('Login error:', err);
          this.error = true;
        }
      },
      resetForm() {
        this.email = "";
        this.password = "";
        this.error = false;
      },
    },
  };
  </script>