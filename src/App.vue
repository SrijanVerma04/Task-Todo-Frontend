<script setup>
import {ref, computed} from 'vue';
import AppHeader from './components/AppHeader.vue';
import HomePage from './components/HomePage.vue';
import LoginPage from './components/LoginPage.vue';
import SignupPage from './components/SignupPage.vue';

const routes = [
  { path: '/', component: HomePage },
  { path: '/auth/login', component: LoginPage },
  { path: '/auth/signup', component: SignupPage },
];

const currentPath = ref(window.location.pathname); // Use pathname for path-based routing

window.addEventListener('popstate', () => {
  currentPath.value = window.location.pathname;
});

const currentView = computed(() => {
  const matchingRoute = routes.find(route => route.path === currentPath.value);
  return matchingRoute ? matchingRoute.component : HomePage;
});
</script>

<template>
  <div id="app">
    <AppHeader :routes="routes" />
    <component :is="currentView" />
  </div>
</template>
