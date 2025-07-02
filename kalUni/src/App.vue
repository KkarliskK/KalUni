<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import AppFooter from './components/AppFooter.vue'
import AppHeader from './components/AppHeader.vue'

const router = useRouter()
const route = useRoute()
const isAuthenticated = ref(false)

onMounted(() => {
  isAuthenticated.value = localStorage.getItem('isAuthenticated') === 'true'
})

const logout = () => {
  localStorage.removeItem('isAuthenticated')
  isAuthenticated.value = false
  router.push('/')
}

const showNavigation = () => {
  return !['/login', '/register'].includes(route.path)
}
</script>

<template>
  <div id="app">
    <!-- Header - Pass isAuthenticated as prop and listen for logout event -->
    <AppHeader 
      v-if="showNavigation()" 
      :is-authenticated="isAuthenticated"
      @logout="logout"
    />
    
    <main :class="{ 'with-nav': showNavigation() }">
      <router-view />
    </main>
    
    <!-- Footer -->
    <AppFooter v-if="showNavigation()" />
  </div>
</template>