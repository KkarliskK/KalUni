<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

interface Props {
  isAuthenticated: boolean
}

const props = defineProps<Props>()

const emit = defineEmits<{
  logout: []
}>()

const router = useRouter()
const isMobileMenuOpen = ref(false)

const logout = () => {
  emit('logout')
}

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}
</script>

<template>
  <!-- Navigation bar -->
  <nav class="sticky top-0 z-50 border-b shadow-sm bg-white/95 backdrop-blur-md border-gray-200/50">
    <div class="px-4 mx-auto max-w-7xl sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <!-- Logo -->
        <div class="flex-shrink-0">
          <router-link 
            to="/" 
            class="flex items-center space-x-2 text-2xl font-bold text-transparent transition-all duration-200 bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text hover:from-indigo-700 hover:to-purple-700"
          >
            KalUnix
          </router-link>
        </div>

        <!-- Desktop Navigation -->
        <div class="items-center hidden space-x-1 md:flex">
          <template v-if="props.isAuthenticated">
            <router-link 
              to="/dashboard" 
              class="flex items-center px-4 py-2 space-x-2 text-sm font-medium text-gray-700 transition-all duration-200 rounded-lg hover:text-indigo-600 hover:bg-indigo-50"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path>
              </svg>
              <span>Dashboard</span>
            </router-link>
            
            <button 
              @click="logout" 
              class="flex items-center px-4 py-2 ml-2 space-x-2 text-sm font-medium text-red-600 transition-all duration-200 rounded-lg hover:text-red-700 hover:bg-red-50"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"></path>
              </svg>
              <span>Logout</span>
            </button>
          </template>
          
          <template v-else>
            <router-link 
              to="/login" 
              class="px-4 py-2 text-sm font-medium text-gray-700 transition-all duration-200 rounded-lg hover:text-indigo-600 hover:bg-indigo-50"
            >
              Login
            </router-link>
            
            <router-link 
              to="/register" 
              class="ml-2 px-4 py-2 text-sm font-medium text-white bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 rounded-lg shadow-md hover:shadow-lg transition-all duration-200 transform hover:-translate-y-0.5"
            >
              Sign Up
            </router-link>
          </template>
        </div>

        <!-- Mobile menu button -->
        <div class="md:hidden">
          <button 
            @click="toggleMobileMenu"
            class="p-2 text-gray-600 transition-all duration-200 rounded-lg hover:text-indigo-600 hover:bg-indigo-50"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path 
                v-if="!isMobileMenuOpen"
                stroke-linecap="round" 
                stroke-linejoin="round" 
                stroke-width="2" 
                d="M4 6h16M4 12h16M4 18h16"
              />
              <path 
                v-else
                stroke-linecap="round" 
                stroke-linejoin="round" 
                stroke-width="2" 
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Mobile Navigation Menu -->
    <div 
      v-if="isMobileMenuOpen" 
      class="border-t md:hidden border-gray-200/50 bg-white/95 backdrop-blur-md"
    >
      <div class="px-4 py-4 space-y-2">
        <template v-if="props.isAuthenticated">
          <router-link 
            to="/dashboard" 
            @click="isMobileMenuOpen = false"
            class="flex items-center px-4 py-3 space-x-3 text-base font-medium text-gray-700 transition-all duration-200 rounded-lg hover:text-indigo-600 hover:bg-indigo-50"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path>
            </svg>
            <span>Dashboard</span>
          </router-link>
          
          <button 
            @click="logout(); isMobileMenuOpen = false" 
            class="flex items-center w-full px-4 py-3 space-x-3 text-base font-medium text-red-600 transition-all duration-200 rounded-lg hover:text-red-700 hover:bg-red-50"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"></path>
            </svg>
            <span>Logout</span>
          </button>
        </template>
        
        <template v-else>
          <router-link 
            to="/login" 
            @click="isMobileMenuOpen = false"
            class="block px-4 py-3 text-base font-medium text-gray-700 transition-all duration-200 rounded-lg hover:text-indigo-600 hover:bg-indigo-50"
          >
            Login
          </router-link>
          
          <router-link 
            to="/register" 
            @click="isMobileMenuOpen = false"
            class="block px-4 py-3 text-base font-medium text-center text-white transition-all duration-200 rounded-lg shadow-md bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700"
          >
            Sign Up
          </router-link>
        </template>
      </div>
    </div>
  </nav>
</template>