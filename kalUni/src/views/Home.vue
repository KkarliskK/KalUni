<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true
  }, 100)
})
</script>

<template>
  <div class="min-h-screen home bg-gradient-to-br from-slate-50 via-white to-indigo-50">
    <section class="relative flex items-center min-h-screen overflow-hidden bg-gradient-to-br from-gray-50 via-blue-50 to-indigo-100">
      <div class="absolute inset-0 bg-white/50"></div>
      
      <!-- Subtle animated background elements -->
      <div class="absolute inset-0 overflow-hidden">
        <div class="absolute w-64 h-64 rounded-full top-1/4 left-1/4 bg-blue-200/30 blur-3xl animate-pulse"></div>
        <div class="absolute delay-1000 rounded-full bottom-1/4 right-1/4 w-80 h-80 bg-indigo-200/20 blur-3xl animate-pulse"></div>
      </div>

      <div class="relative z-10 px-4 py-20 mx-auto max-w-7xl sm:px-6 lg:px-8">
        <div class="grid items-center grid-cols-1 gap-12 lg:grid-cols-2 lg:gap-20">
          <!-- Left side - Text content -->
          <div class="text-left">
            <h1 class="mb-6 text-4xl font-bold leading-tight text-gray-900 sm:text-5xl lg:text-6xl"
                :class="{ 'animate-fade-in-up': isVisible }">
              Take Control of Your
              <span class="block text-transparent bg-gradient-to-r from-blue-600 to-indigo-600 bg-clip-text">
                Financial Future
              </span>
            </h1>

            <p class="max-w-lg mb-8 text-xl leading-relaxed text-gray-600"
               :class="{ 'animate-fade-in-up': isVisible }" style="animation-delay: 0.2s">
              Smart expense tracking, AI-powered receipt scanning, and intelligent insights to help you make better financial decisions.
            </p>

            <div class="flex flex-col gap-4 mb-12 sm:flex-row"
                 :class="{ 'animate-fade-in-up': isVisible }" style="animation-delay: 0.4s">
              <router-link 
                to="/register" 
                class="inline-flex items-center justify-center px-8 py-4 text-lg font-semibold text-white transition-all duration-300 transform shadow-lg group bg-gradient-to-r from-blue-600 to-indigo-600 rounded-xl hover:shadow-xl hover:-translate-y-1 hover:scale-105"
              >
                <span class="flex items-center">
                  Get Started Free
                  <svg class="w-5 h-5 ml-2 transition-transform duration-200 group-hover:translate-x-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"></path>
                  </svg>
                </span>
              </router-link>
              
              <router-link 
                to="/login" 
                class="inline-flex items-center justify-center px-8 py-4 text-lg font-semibold text-gray-700 transition-all duration-300 bg-white border-2 border-gray-200 group rounded-xl hover:bg-gray-50 hover:border-gray-300"
              >
                <span class="flex items-center">
                  <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 16l-4-4m0 0l4-4m-4 4h14m-5 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"></path>
                  </svg>
                  Sign In
                </span>
              </router-link>
            </div>

            <!-- Trust indicators -->
            <div class="flex items-center space-x-8 text-sm text-gray-500"
                 :class="{ 'animate-fade-in-up': isVisible }" style="animation-delay: 0.6s">
              <div class="flex items-center space-x-2">
                <div class="flex -space-x-2">
                  <div class="w-8 h-8 border-2 border-white rounded-full bg-gradient-to-r from-blue-400 to-blue-600"></div>
                  <div class="w-8 h-8 border-2 border-white rounded-full bg-gradient-to-r from-green-400 to-green-600"></div>
                  <div class="w-8 h-8 border-2 border-white rounded-full bg-gradient-to-r from-purple-400 to-purple-600"></div>
                </div>
                <span>10,000+ happy users</span>
              </div>
              <div class="flex items-center space-x-1">
                <div class="flex space-x-1">
                  <svg class="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 20 20">
                    <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                  </svg>
                  <svg class="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 20 20">
                    <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                  </svg>
                  <svg class="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 20 20">
                    <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                  </svg>
                  <svg class="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 20 20">
                    <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                  </svg>
                  <svg class="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 20 20">
                    <path d="M10 15l-5.878 3.09 1.123-6.545L.489 6.91l6.572-.955L10 0l2.939 5.955 6.572.955-4.756 4.635 1.123 6.545z"/>
                  </svg>
                </div>
                <span>4.9/5 rating</span>
              </div>
            </div>
          </div>

          <!-- Right side - Dashboard mockup image -->
          <div class="relative"
               :class="{ 'animate-fade-in-up': isVisible }" style="animation-delay: 0.3s">
            <div class="relative p-6 transition-transform duration-500 transform bg-white shadow-2xl rounded-2xl rotate-2 hover:rotate-0">
              <div class="flex items-center justify-between mb-6">
                <div class="flex items-center space-x-3">
                  <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-500 to-indigo-600"></div>
                  <span class="font-semibold text-gray-900">KalUnix Dashboard</span>
                </div>
                <div class="flex space-x-2">
                  <div class="w-3 h-3 bg-red-400 rounded-full"></div>
                  <div class="w-3 h-3 bg-yellow-400 rounded-full"></div>
                  <div class="w-3 h-3 bg-green-400 rounded-full"></div>
                </div>
              </div>
              
              <div class="p-6 mb-6 text-white bg-gradient-to-r from-blue-500 to-indigo-600 rounded-xl">
                <p class="mb-2 text-sm text-blue-100">Total Balance</p>
                <p class="text-3xl font-bold">$12,847.50</p>
                <div class="flex items-center mt-2 text-sm text-blue-100">
                  <svg class="w-4 h-4 mr-1" fill="currentColor" viewBox="0 0 20 20">
                    <path fill-rule="evenodd" d="M12 7a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0V8.414l-4.293 4.293a1 1 0 01-1.414 0L8 10.414l-4.293 4.293a1 1 0 01-1.414-1.414l5-5a1 1 0 011.414 0L11 10.586 14.586 7H12z" clip-rule="evenodd"/>
                  </svg>
                  +$1,245 this month
                </div>
              </div>
              
              <div class="space-y-4">
                <h3 class="mb-4 font-semibold text-gray-900">Recent Transactions</h3>
                <div class="flex items-center justify-between">
                  <div class="flex items-center space-x-3">
                    <div class="flex items-center justify-center w-10 h-10 bg-green-100 rounded-full">
                      <svg class="w-5 h-5 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"></path>
                      </svg>
                    </div>
                    <div>
                      <p class="font-medium text-gray-900">Coffee Shop</p>
                      <p class="text-sm text-gray-500">Today, 9:30 AM</p>
                    </div>
                  </div>
                  <span class="font-semibold text-red-600">-$4.50</span>
                </div>
                <div class="flex items-center justify-between">
                  <div class="flex items-center space-x-3">
                    <div class="flex items-center justify-center w-10 h-10 bg-blue-100 rounded-full">
                      <svg class="w-5 h-5 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1"></path>
                      </svg>
                    </div>
                    <div>
                      <p class="font-medium text-gray-900">Salary Deposit</p>
                      <p class="text-sm text-gray-500">Yesterday, 2:00 PM</p>
                    </div>
                  </div>
                  <span class="font-semibold text-green-600">+$3,200.00</span>
                </div>
              </div>
            </div>
            
            <!-- Floating elements -->
            <div class="absolute p-4 transition-transform duration-500 transform bg-white rounded-lg shadow-lg -top-4 -right-4 -rotate-12 hover:rotate-0">
              <div class="flex items-center space-x-2">
                <div class="w-3 h-3 bg-green-400 rounded-full animate-pulse"></div>
                <span class="text-sm font-medium text-gray-700">AI Active</span>
              </div>
            </div>
            
            <div class="absolute p-4 text-white transition-transform duration-500 transform rounded-lg shadow-lg -bottom-6 -left-6 bg-gradient-to-r from-green-400 to-blue-500 rotate-6 hover:rotate-0">
              <p class="text-sm font-medium">Receipt Scanned!</p>
              <p class="text-xs opacity-90">Auto-categorized</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section class="py-20 bg-white sm:py-32">
      <div class="px-4 mx-auto max-w-7xl sm:px-6 lg:px-8">
        <div class="mb-16 text-center sm:mb-20">
          <h2 class="mb-6 text-3xl font-bold text-gray-900 sm:text-4xl lg:text-5xl">
            Powerful Features for
            <span class="text-transparent bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text">
              Smart Finance
            </span>
          </h2>
          <p class="max-w-2xl mx-auto text-xl text-gray-600">
            Everything you need to manage your finances efficiently and make informed decisions
          </p>
        </div>

        <!-- Features grid -->
        <div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-3 lg:gap-12">
          <!-- Feature 1 -->
          <div class="relative p-8 transition-all duration-300 border border-gray-100 shadow-lg group bg-gradient-to-br from-white to-gray-50 rounded-2xl hover:shadow-2xl hover:-translate-y-2">
            <div class="absolute inset-0 transition-opacity duration-300 opacity-0 bg-gradient-to-br from-blue-500/5 to-purple-500/5 rounded-2xl group-hover:opacity-100"></div>
            <div class="relative z-10">
              <div class="flex items-center justify-center w-16 h-16 mb-6 transition-transform duration-300 bg-gradient-to-br from-blue-500 to-indigo-600 rounded-2xl group-hover:scale-110">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path>
                </svg>
              </div>
              <h3 class="mb-4 text-2xl font-bold text-gray-900 transition-colors duration-300 group-hover:text-indigo-600">
                Track Expenses
              </h3>
              <p class="leading-relaxed text-gray-600">
                Monitor your spending and income in real-time with beautiful charts and detailed categorization
              </p>
            </div>
          </div>

          <!-- Feature 2 -->
          <div class="relative p-8 transition-all duration-300 border border-gray-100 shadow-lg group bg-gradient-to-br from-white to-gray-50 rounded-2xl hover:shadow-2xl hover:-translate-y-2">
            <div class="absolute inset-0 transition-opacity duration-300 opacity-0 bg-gradient-to-br from-green-500/5 to-emerald-500/5 rounded-2xl group-hover:opacity-100"></div>
            <div class="relative z-10">
              <div class="flex items-center justify-center w-16 h-16 mb-6 transition-transform duration-300 bg-gradient-to-br from-green-500 to-emerald-600 rounded-2xl group-hover:scale-110">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.5 2.5 0 11-5 0 2.5 2.5 0 015 0z"></path>
                </svg>
              </div>
              <h3 class="mb-4 text-2xl font-bold text-gray-900 transition-colors duration-300 group-hover:text-green-600">
                AI Receipt Scanning
              </h3>
              <p class="leading-relaxed text-gray-600">
                Automatically extract data from receipts using advanced AI technology - just snap a photo!
              </p>
            </div>
          </div>

          <!-- Feature 3 -->
          <div class="relative p-8 transition-all duration-300 border border-gray-100 shadow-lg group bg-gradient-to-br from-white to-gray-50 rounded-2xl hover:shadow-2xl hover:-translate-y-2 md:col-span-2 lg:col-span-1">
            <div class="absolute inset-0 transition-opacity duration-300 opacity-0 bg-gradient-to-br from-purple-500/5 to-pink-500/5 rounded-2xl group-hover:opacity-100"></div>
            <div class="relative z-10">
              <div class="flex items-center justify-center w-16 h-16 mb-6 transition-transform duration-300 bg-gradient-to-br from-purple-500 to-pink-600 rounded-2xl group-hover:scale-110">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                </svg>
              </div>
              <h3 class="mb-4 text-2xl font-bold text-gray-900 transition-colors duration-300 group-hover:text-purple-600">
                Financial Insights
              </h3>
              <p class="leading-relaxed text-gray-600">
                Get detailed reports and analytics on your finances with AI-powered insights and recommendations
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="py-20 bg-gradient-to-br from-slate-50 via-white to-indigo-50">
      <div class="max-w-4xl px-4 mx-auto text-center sm:px-6 lg:px-8">
        <h2 class="mb-6 text-3xl font-bold sm:text-4xl lg:text-5xl">
          Ready to Take Control of Your Finances?
        </h2>
        <p class="mb-8 text-xl leading-relaxed text-gray-600 animate-fade-in-up">
          Join thousands of users who are already making smarter financial decisions
        </p>
        <router-link 
            to="/register" 
            class="inline-flex items-center justify-center px-8 py-4 text-lg font-semibold text-white transition-all duration-300 transform shadow-lg group bg-gradient-to-r from-blue-600 to-indigo-600 rounded-xl hover:shadow-xl hover:-translate-y-1 hover:scale-105"
            >
            <span class="flex items-center">
                Get started free
                <svg class="w-5 h-5 ml-2 transition-transform duration-200 group-hover:translate-x-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"></path>
                </svg>
            </span>
        </router-link>
      </div>
    </section>
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

@keyframes gradient-x {
  0%, 100% {
    background-size: 200% 200%;
    background-position: left center;
  }
  50% {
    background-size: 200% 200%;
    background-position: right center;
  }
}

.animate-fade-in-up {
  animation: fade-in-up 0.8s ease-out forwards;
}

.animate-gradient-x {
  animation: gradient-x 3s ease infinite;
}
</style>