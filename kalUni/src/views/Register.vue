<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'

const isVisible = ref(false)
const firstName = ref('')
const lastName = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const agreeToTerms = ref(false)
const isLoading = ref(false)

const passwordStrength = computed(() => {
  const pwd = password.value
  let strength = 0
  if (pwd.length >= 8) strength++
  if (/[a-z]/.test(pwd)) strength++
  if (/[A-Z]/.test(pwd)) strength++
  if (/[0-9]/.test(pwd)) strength++
  if (/[^A-Za-z0-9]/.test(pwd)) strength++
  return strength
})

const passwordsMatch = computed(() => {
  return password.value === confirmPassword.value
})

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true
  }, 100)
})

const handleRegister = async () => {
  if (!passwordsMatch.value) {
    alert('Passwords do not match!')
    return
  }
  if (!agreeToTerms.value) {
    alert('Please agree to the terms and conditions!')
    return
  }
  
  isLoading.value = true
  // Simulate API call
  setTimeout(() => {
    isLoading.value = false
    // Handle registration logic here
    console.log('Registration attempt:', { 
      firstName: firstName.value, 
      lastName: lastName.value,
      email: email.value, 
      password: password.value 
    })
  }, 1500)
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-50 via-white to-indigo-50">
    <div class="relative flex items-center justify-center min-h-screen py-12 overflow-hidden">
      <!-- Background elements -->
      <div class="absolute inset-0 bg-white/50"></div>
      <div class="absolute inset-0 overflow-hidden">
        <div class="absolute w-64 h-64 rounded-full top-1/4 left-1/4 bg-blue-200/30 blur-3xl animate-pulse"></div>
        <div class="absolute delay-1000 rounded-full bottom-1/4 right-1/4 w-80 h-80 bg-indigo-200/20 blur-3xl animate-pulse"></div>
      </div>

      <div class="relative z-10 w-full max-w-lg px-4">
        <!-- Back to home link -->
        <div class="mb-8 text-center"
             :class="{ 'animate-fade-in-up': isVisible }">
          <router-link 
            to="/" 
            class="inline-flex items-center text-sm font-medium text-gray-600 transition-colors duration-200 hover:text-indigo-600"
          >
            <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path>
            </svg>
            Back to KalUnix
          </router-link>
        </div>

        <!-- Registration form -->
        <div class="p-8 transition-all duration-500 transform bg-white shadow-2xl rounded-2xl"
             :class="{ 'animate-fade-in-up': isVisible }" style="animation-delay: 0.2s">
          
          <!-- Header -->
          <div class="mb-8 text-center">
            <div class="flex items-center justify-center mb-4">
              <div class="w-12 h-12 rounded-lg bg-gradient-to-br from-blue-500 to-indigo-600"></div>
            </div>
            <h1 class="mb-2 text-3xl font-bold text-gray-900">Create Account</h1>
            <p class="text-gray-600">Join KalUnix and take control of your finances</p>
          </div>

          <!-- Form -->
          <form @submit.prevent="handleRegister" class="space-y-6">
            <!-- Name fields -->
            <div class="grid grid-cols-2 gap-4">
              <div>
                <label for="firstName" class="block mb-2 text-sm font-medium text-gray-700">
                  First Name
                </label>
                <input
                  id="firstName"
                  v-model="firstName"
                  type="text"
                  required
                  class="w-full px-4 py-3 text-gray-900 placeholder-gray-500 transition-all duration-200 border border-gray-200 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/20 focus:outline-none"
                  placeholder="John"
                />
              </div>
              <div>
                <label for="lastName" class="block mb-2 text-sm font-medium text-gray-700">
                  Last Name
                </label>
                <input
                  id="lastName"
                  v-model="lastName"
                  type="text"
                  required
                  class="w-full px-4 py-3 text-gray-900 placeholder-gray-500 transition-all duration-200 border border-gray-200 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/20 focus:outline-none"
                  placeholder="Doe"
                />
              </div>
            </div>

            <!-- Email -->
            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-gray-700">
                Email Address
              </label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                  <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207"></path>
                  </svg>
                </div>
                <input
                  id="email"
                  v-model="email"
                  type="email"
                  required
                  class="w-full py-3 pl-10 pr-4 text-gray-900 placeholder-gray-500 transition-all duration-200 border border-gray-200 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/20 focus:outline-none"
                  placeholder="john@example.com"
                />
              </div>
            </div>

            <!-- Password -->
            <div>
              <label for="password" class="block mb-2 text-sm font-medium text-gray-700">
                Password
              </label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                  <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"></path>
                  </svg>
                </div>
                <input
                  id="password"
                  v-model="password"
                  type="password"
                  required
                  class="w-full py-3 pl-10 pr-4 text-gray-900 placeholder-gray-500 transition-all duration-200 border border-gray-200 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/20 focus:outline-none"
                  placeholder="Create a strong password"
                />
              </div>
              
              <!-- Password strength indicator -->
              <div v-if="password" class="mt-2">
                <div class="flex items-center space-x-2">
                  <div class="flex flex-1 space-x-1">
                    <div 
                      v-for="i in 5" 
                      :key="i"
                      class="flex-1 h-1 transition-colors duration-200 rounded-full"
                      :class="{
                        'bg-red-400': i <= passwordStrength && passwordStrength <= 2,
                        'bg-yellow-400': i <= passwordStrength && passwordStrength === 3,
                        'bg-green-400': i <= passwordStrength && passwordStrength >= 4,
                        'bg-gray-200': i > passwordStrength
                      }"
                    ></div>
                  </div>
                  <span class="text-xs font-medium" :class="{
                    'text-red-600': passwordStrength <= 2,
                    'text-yellow-600': passwordStrength === 3,
                    'text-green-600': passwordStrength >= 4
                  }">
                    {{ passwordStrength <= 2 ? 'Weak' : passwordStrength === 3 ? 'Good' : 'Strong' }}
                  </span>
                </div>
              </div>
            </div>

            <!-- Confirm Password -->
            <div>
              <label for="confirmPassword" class="block mb-2 text-sm font-medium text-gray-700">
                Confirm Password
              </label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                  <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                  </svg>
                </div>
                <input
                  id="confirmPassword"
                  v-model="confirmPassword"
                  type="password"
                  required
                  class="w-full py-3 pl-10 pr-4 text-gray-900 placeholder-gray-500 transition-all duration-200 border border-gray-200 rounded-xl focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/20 focus:outline-none"
                  :class="{
                    'border-red-300 focus:border-red-500 focus:ring-red-500/20': confirmPassword && !passwordsMatch,
                    'border-green-300 focus:border-green-500 focus:ring-green-500/20': confirmPassword && passwordsMatch
                  }"
                  placeholder="Confirm your password"
                />
              </div>
              <div v-if="confirmPassword && !passwordsMatch" class="mt-1 text-sm text-red-600">
                Passwords do not match
              </div>
            </div>

            <!-- Terms and conditions -->
            <div class="flex items-start space-x-3">
              <input
                id="agreeToTerms"
                v-model="agreeToTerms"
                type="checkbox"
                class="w-4 h-4 mt-0.5 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
              />
              <label for="agreeToTerms" class="text-sm text-gray-600">
                I agree to the 
                <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500">Terms of Service</a>
                and 
                <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500">Privacy Policy</a>
              </label>
            </div>

            <!-- Submit button -->
            <button
              type="submit"
              :disabled="isLoading || !agreeToTerms"
              class="relative w-full py-3 text-lg font-semibold text-white transition-all duration-300 transform shadow-lg bg-gradient-to-r from-blue-600 to-indigo-600 rounded-xl hover:shadow-xl hover:-translate-y-1 disabled:opacity-50 disabled:cursor-not-allowed disabled:transform-none"
            >
              <span v-if="!isLoading" class="flex items-center justify-center">
                Create Account
                <svg class="w-5 h-5 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"></path>
                </svg>
              </span>
              <span v-else class="flex items-center justify-center">
                <svg class="w-5 h-5 mr-2 animate-spin" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                Creating Account...
              </span>
            </button>
          </form>

          <!-- Divider -->
          <div class="my-8">
            <div class="relative">
              <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-gray-200"></div>
              </div>
              <div class="relative flex justify-center text-sm">
                <span class="px-4 text-gray-500 bg-white">Or continue with</span>
              </div>
            </div>
          </div>

          <!-- Social login buttons -->
          <div class="grid grid-cols-2 gap-4">
            <button class="flex items-center justify-center py-3 transition-all duration-200 border border-gray-200 rounded-xl hover:bg-gray-50 hover:border-gray-300">
              <svg class="w-5 h-5 mr-2" viewBox="0 0 24 24">
                <path fill="#4285f4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
                <path fill="#34a853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
                <path fill="#fbbc05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
                <path fill="#ea4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
              </svg>
              Google
            </button>
            <button class="flex items-center justify-center py-3 transition-all duration-200 border border-gray-200 rounded-xl hover:bg-gray-50 hover:border-gray-300">
              <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24">
                <path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"/>
              </svg>
              Twitter
            </button>
          </div>

          <!-- Sign up link -->
          <div class="mt-8 text-center">
            <p class="text-gray-600">
              Already have an account?
              <router-link to="/register" class="font-medium text-indigo-600 transition-colors duration-200 hover:text-indigo-500">
                Sign in here
              </router-link>
            </p>
          </div>
        </div>

        <!-- Trust indicators -->
        <div class="mt-8 text-center"
             :class="{ 'animate-fade-in-up': isVisible }" style="animation-delay: 0.4s">
          <div class="flex items-center justify-center space-x-6 text-sm text-gray-500">
            <div class="flex items-center space-x-1">
              <svg class="w-4 h-4 text-green-500" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"/>
              </svg>
              <span>SSL Secured</span>
            </div>
            <div class="flex items-center space-x-1">
              <svg class="w-4 h-4 text-blue-500" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z" clip-rule="evenodd"/>
              </svg>
              <span>Bank-level Security</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in-up {
  animation: fade-in-up 0.8s ease-out forwards;
}
</style>