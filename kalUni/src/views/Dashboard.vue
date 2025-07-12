<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import ReceiptUploader from '../components/ReceiptUploader.vue';

interface Transaction {
  id: string
  amount: number
  description: string
  category: string
  date: string
  type: 'income' | 'expense'
}

const transactions = ref<Transaction[]>([])
const loading = ref(true)

const totalIncome = computed(() => {
  return transactions.value
    .filter(t => t.type === 'income')
    .reduce((sum, t) => sum + t.amount, 0)
})

const totalExpenses = computed(() => {
  return transactions.value
    .filter(t => t.type === 'expense')
    .reduce((sum, t) => sum + t.amount, 0)
})

const balance = computed(() => totalIncome.value - totalExpenses.value)

const recentTransactions = computed(() => {
  return transactions.value
    .sort((a, b) => new Date(b.date).getTime() - new Date(a.date).getTime())
    .slice(0, 5)
})

const loadTransactions = () => {
  const stored = localStorage.getItem('transactions')
  if (stored) {
    transactions.value = JSON.parse(stored)
  } else {
    transactions.value = [
      {
        id: '1',
        amount: 2500,
        description: 'Salary',
        category: 'Income',
        date: '2024-01-15',
        type: 'income'
      },
      {
        id: '2',
        amount: 45.50,
        description: 'Groceries',
        category: 'Food',
        date: '2024-01-14',
        type: 'expense'
      },
      {
        id: '3',
        amount: 120,
        description: 'Gas Bill',
        category: 'Utilities',
        date: '2024-01-13',
        type: 'expense'
      }
    ]
  }
  loading.value = false
}

const saveTransactions = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

const handleTransactionAdded = (transaction: Transaction) => {
  transactions.value.push(transaction)
  saveTransactions()
}

onMounted(() => {
  loadTransactions()
})

const formatCurrency = (amount: number): string => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(amount)
}
</script>

<template>
  <div class="px-4 py-8 mx-auto max-w-7xl sm:px-6 lg:px-8">
    <!-- Header -->
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-900">Your Financial Dashboard</h1>
      <p class="mt-2 text-gray-600">Overview of your financial activity</p>
    </div>
    
    <!-- Loading state -->
    <div v-if="loading" class="flex items-center justify-center py-20">
      <div class="w-12 h-12 border-b-2 border-blue-600 rounded-full animate-spin"></div>
      <span class="ml-3 text-lg text-gray-600">Loading your financial data...</span>
    </div>
    
    <!-- Main content -->
    <div v-else class="space-y-8">
      <!-- Summary cards -->
      <div class="grid grid-cols-1 gap-6 md:grid-cols-3">
        <!-- Income Card -->
        <div class="p-6 bg-white border border-gray-200 rounded-lg shadow-sm">
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <div class="flex items-center justify-center w-8 h-8 bg-green-100 rounded-full">
                <svg class="w-4 h-4 text-green-600" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-8.707l-3-3a1 1 0 00-1.414 0l-3 3a1 1 0 001.414 1.414L9 9.414V13a1 1 0 102 0V9.414l1.293 1.293a1 1 0 001.414-1.414z" clip-rule="evenodd"/>
                </svg>
              </div>
            </div>
            <div class="flex-1 ml-4">
              <p class="text-sm font-medium tracking-wide text-gray-500 uppercase">Total Income</p>
              <p class="text-2xl font-bold text-gray-900">{{ formatCurrency(totalIncome) }}</p>
            </div>
          </div>
        </div>

        <!-- Expenses Card -->
        <div class="p-6 bg-white border border-gray-200 rounded-lg shadow-sm">
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <div class="flex items-center justify-center w-8 h-8 bg-red-100 rounded-full">
                <svg class="w-4 h-4 text-red-600" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-10.293l-3-3a1 1 0 00-1.414 0l-3 3a1 1 0 001.414 1.414L9 6.414V10a1 1 0 102 0V6.414l1.293 1.293a1 1 0 001.414-1.414z" clip-rule="evenodd"/>
                </svg>
              </div>
            </div>
            <div class="flex-1 ml-4">
              <p class="text-sm font-medium tracking-wide text-gray-500 uppercase">Total Expenses</p>
              <p class="text-2xl font-bold text-gray-900">{{ formatCurrency(totalExpenses) }}</p>
            </div>
          </div>
        </div>

        <!-- Balance Card -->
        <div class="p-6 bg-white border border-gray-200 rounded-lg shadow-sm">
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <div :class="[
                'w-8 h-8 rounded-full flex items-center justify-center',
                balance >= 0 ? 'bg-blue-100' : 'bg-red-100'
              ]">
                <svg :class="[
                  'w-4 h-4',
                  balance >= 0 ? 'text-blue-600' : 'text-red-600'
                ]" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M8.433 7.418c.155-.103.346-.196.567-.267v1.698a2.305 2.305 0 01-.567-.267C8.07 8.34 8 8.114 8 8c0-.114.07-.34.433-.582zM11 12.849v-1.698c.22.071.412.164.567.267.364.243.433.468.433.582 0 .114-.07.34-.433.582a2.305 2.305 0 01-.567.267z"/>
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-13a1 1 0 10-2 0v.092a4.535 4.535 0 00-1.676.662C6.602 6.234 6 7.009 6 8c0 .99.602 1.765 1.324 2.246.48.32 1.054.545 1.676.662v1.941c-.391-.127-.68-.317-.843-.504a1 1 0 10-1.51 1.31c.562.649 1.413 1.076 2.353 1.253V15a1 1 0 102 0v-.092a4.535 4.535 0 001.676-.662C13.398 13.766 14 12.991 14 12c0-.99-.602-1.765-1.324-2.246A4.535 4.535 0 0011 9.092V7.151c.391.127.68.317.843.504a1 1 0 101.511-1.31c-.563-.649-1.413-1.076-2.354-1.253V5z" clip-rule="evenodd"/>
                </svg>
              </div>
            </div>
            <div class="flex-1 ml-4">
              <p class="text-sm font-medium tracking-wide text-gray-500 uppercase">Balance</p>
              <p :class="[
                'text-2xl font-bold',
                balance >= 0 ? 'text-gray-900' : 'text-red-600'
              ]">{{ formatCurrency(balance) }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Receipt Uploader -->
      <ReceiptUploader @transaction-added="handleTransactionAdded" />
      
      <!-- Recent transactions -->
      <div class="bg-white border border-gray-200 rounded-lg shadow-sm">
        <div class="px-6 py-4 border-b border-gray-200">
          <h2 class="text-lg font-medium text-gray-900">Recent Transactions</h2>
        </div>
        
        <div v-if="recentTransactions.length === 0" class="px-6 py-20 text-center">
          <svg class="w-12 h-12 mx-auto text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v10a2 2 0 002 2h8a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/>
          </svg>
          <h3 class="mt-2 text-sm font-medium text-gray-900">No transactions</h3>
          <p class="mt-1 text-sm text-gray-500">Get started by uploading a receipt or adding your first transaction.</p>
        </div>
        
        <div v-else class="divide-y divide-gray-200">
          <div 
            v-for="transaction in recentTransactions" 
            :key="transaction.id"
            class="px-6 py-4 transition-colors duration-150 hover:bg-gray-50"
          >
            <div class="flex items-center justify-between">
              <div class="flex-1 min-w-0">
                <div class="flex items-center space-x-3">
                  <div :class="[
                    'flex-shrink-0 w-2 h-2 rounded-full',
                    transaction.type === 'income' ? 'bg-green-400' : 'bg-red-400'
                  ]"></div>
                  <div class="flex-1 min-w-0">
                    <p class="text-sm font-medium text-gray-900 truncate">
                      {{ transaction.description }}
                    </p>
                    <p class="text-sm text-gray-500">
                      {{ transaction.category }} • {{ transaction.date }}
                    </p>
                  </div>
                </div>
              </div>
              <div class="flex-shrink-0">
                <span :class="[
                  'inline-flex items-center px-2.5 py-0.5 rounded-full text-sm font-medium',
                  transaction.type === 'income' 
                    ? 'text-green-800 bg-green-100' 
                    : 'text-red-800 bg-red-100'
                ]">
                  {{ transaction.type === 'income' ? '+' : '-' }}{{ formatCurrency(Math.abs(transaction.amount)) }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>