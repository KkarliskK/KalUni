<template>
  <div class="p-6 bg-white border border-gray-200 rounded-lg shadow-sm">
    <div class="mb-4">
      <h3 class="text-lg font-medium text-gray-900">Upload Receipt</h3>
      <p class="text-sm text-gray-500">Upload a receipt image to automatically extract transaction details</p>
    </div>

    <!-- File Upload Area -->
    <div class="mb-4">
      <div 
        @drop="handleDrop"
        @dragover="handleDragOver"
        @dragleave="handleDragLeave"
        :class="[
          'border-2 border-dashed rounded-lg p-6 text-center transition-colors duration-200',
          isDragOver ? 'border-blue-500 bg-blue-50' : 'border-gray-300 hover:border-gray-400'
        ]"
      >
        <div class="space-y-2">
          <svg class="w-12 h-12 mx-auto text-gray-400" stroke="currentColor" fill="none" viewBox="0 0 48 48">
            <path d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <div class="text-sm text-gray-600">
            <label class="cursor-pointer">
              <span class="font-medium text-blue-600 hover:text-blue-500">Click to upload</span>
              or drag and drop
              <input 
                ref="fileInput"
                type="file" 
                accept="image/*" 
                @change="handleFileSelect"
                class="hidden"
              >
            </label>
          </div>
          <p class="text-xs text-gray-500">PNG, JPG, JPEG up to 10MB</p>
        </div>
      </div>
    </div>

    <!-- Preview and Processing -->
    <div v-if="selectedFile" class="mb-4">
      <div class="flex items-center p-4 space-x-4 rounded-lg bg-gray-50">
        <img 
          v-if="imagePreview" 
          :src="imagePreview" 
          alt="Receipt preview" 
          class="object-cover w-20 h-20 border border-gray-200 rounded-md"
        >
        <div class="flex-1">
          <p class="text-sm font-medium text-gray-900">{{ selectedFile.name }}</p>
          <p class="text-xs text-gray-500">{{ formatFileSize(selectedFile.size) }}</p>
        </div>
        <button 
          @click="removeFile"
          class="text-red-500 hover:text-red-700"
        >
          <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"/>
          </svg>
        </button>
      </div>
    </div>

    <!-- Processing Status -->
    <div v-if="isProcessing" class="p-4 mb-4 rounded-lg bg-blue-50">
      <div class="flex items-center space-x-3">
        <div class="w-5 h-5 border-b-2 border-blue-600 rounded-full animate-spin"></div>
        <span class="text-sm text-blue-800">{{ processingStatus }}</span>
      </div>
    </div>

    <!-- Error Message -->
    <div v-if="errorMessage" class="p-4 mb-4 rounded-lg bg-red-50">
      <div class="flex items-center space-x-2">
        <svg class="w-5 h-5 text-red-500" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd"/>
        </svg>
        <span class="text-sm text-red-800">{{ errorMessage }}</span>
      </div>
    </div>

    <!-- Success Message -->
    <div v-if="successMessage" class="p-4 mb-4 rounded-lg bg-green-50">
      <div class="flex items-center space-x-2">
        <svg class="w-5 h-5 text-green-500" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"/>
        </svg>
        <span class="text-sm text-green-800">{{ successMessage }}</span>
      </div>
    </div>

    <!-- Extracted Data Form -->
    <div v-if="extractedData" class="p-4 mb-4 rounded-lg bg-green-50">
      <h4 class="mb-3 text-sm font-medium text-green-800">Extracted Information</h4>
      <div class="space-y-3">
        <div>
          <label class="block mb-1 text-xs font-medium text-gray-700">Amount (€)</label>
          <input 
            v-model="extractedData.amount"
            type="number"
            step="0.01"
            class="w-full px-3 py-2 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          >
        </div>
        <div>
          <label class="block mb-1 text-xs font-medium text-gray-700">Description</label>
          <input 
            v-model="extractedData.description"
            type="text"
            class="w-full px-3 py-2 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          >
        </div>
        <div>
          <label class="block mb-1 text-xs font-medium text-gray-700">Category</label>
            <select 
              v-model="extractedData.category"
              class="w-full px-3 py-2 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
            >
              <option value="Pārtika">Pārtika</option>
              <option value="Transports">Transports</option>
              <option value="Iepirkšanās">Iepirkšanās</option>
              <option value="Veselība">Veselība</option>
              <option value="Izklaide">Izklaide</option>
              <option value="Komunālie">Komunālie</option>
              <option value="Cits">Cits</option>
            </select>
        </div>
        <div>
          <label class="block mb-1 text-xs font-medium text-gray-700">Date</label>
          <input 
            v-model="extractedData.date"
            type="date"
            class="w-full px-3 py-2 text-sm border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          >
        </div>
      </div>
    </div>

    <!-- Action Buttons -->
    <div class="flex space-x-3">
      <button 
        @click="processReceipt"
        :disabled="!selectedFile || isProcessing"
        class="flex-1 px-4 py-2 text-sm font-medium text-white transition-colors duration-200 bg-blue-600 rounded-md hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed"
      >
        {{ isProcessing ? 'Processing...' : 'Extract Data' }}
      </button>
      <button 
        v-if="extractedData"
        @click="addTransaction"
        class="flex-1 px-4 py-2 text-sm font-medium text-white transition-colors duration-200 bg-green-600 rounded-md hover:bg-green-700"
      >
        Add Transaction
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, nextTick } from 'vue'
import { createWorker } from 'tesseract.js'

// Types
interface ExtractedData {
  amount: number
  description: string
  category: string
  date: string
}

// Reactive variables
const selectedFile = ref<File | null>(null)
const imagePreview = ref<string | null>(null)
const isDragOver = ref(false)
const isProcessing = ref(false)
const processingStatus = ref('Initializing...')
const errorMessage = ref<string | null>(null)
const successMessage = ref<string | null>(null)
const extractedData = ref<ExtractedData | null>(null)
const fileInput = ref<HTMLInputElement | null>(null)

// Emit events to parent component
const emit = defineEmits<{
  transactionAdded: [data: ExtractedData]
}>()

// File handling methods
const handleDrop = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = false
  
  const files = e.dataTransfer?.files
  if (files && files.length > 0) {
    handleFileSelection(files[0])
  }
}

const handleDragOver = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = true
}

const handleDragLeave = () => {
  isDragOver.value = false
}

const handleFileSelect = (e: Event) => {
  const target = e.target as HTMLInputElement
  if (target.files && target.files.length > 0) {
    handleFileSelection(target.files[0])
  }
}

const handleFileSelection = (file: File) => {
  // Reset previous states
  clearMessages()
  extractedData.value = null
  
  // Validate file
  if (!file.type.startsWith('image/')) {
    errorMessage.value = 'Please select a valid image file (PNG, JPG, JPEG)'
    return
  }
  
  if (file.size > 10 * 1024 * 1024) { // 10MB
    errorMessage.value = 'File size must be less than 10MB'
    return
  }
  
  selectedFile.value = file
  
  // Create preview
  const reader = new FileReader()
  reader.onload = (e) => {
    imagePreview.value = e.target?.result as string
  }
  reader.readAsDataURL(file)
}

const removeFile = () => {
  selectedFile.value = null
  imagePreview.value = null
  extractedData.value = null
  clearMessages()
  
  // Reset file input
  if (fileInput.value) {
    fileInput.value.value = ''
  }
}

const clearMessages = () => {
  errorMessage.value = null
  successMessage.value = null
}

// Utility functions
const formatFileSize = (bytes: number): string => {
  if (bytes === 0) return '0 Bytes'
  const k = 1024
  const sizes = ['Bytes', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
}

// Main processing function
const processReceipt = async () => {
  if (!selectedFile.value) {
    errorMessage.value = 'Please select a file first'
    return
  }
  
  clearMessages()
  isProcessing.value = true
  processingStatus.value = 'Processing receipt...'
  
  try {
    const extractedText = await tryOCRApproaches(selectedFile.value)
    
    if (!extractedText || extractedText.trim() === '') {
      throw new Error('No text could be extracted from the receipt')
    }
    
    processingStatus.value = 'Analyzing extracted text...'
    
    const parsedData = parseReceiptText(extractedText)
    extractedData.value = parsedData
    
    successMessage.value = 'Receipt processed successfully! Please review and edit the extracted information.'
    
  } catch (error) {
    console.error('Processing error:', error)
    errorMessage.value = `Processing failed: ${error instanceof Error ? error.message : 'Unknown error'}`
  } finally {
    isProcessing.value = false
  }
}

const addTransaction = () => {
  if (!extractedData.value) return
  
  // Validate extracted data
  if (!extractedData.value.amount || extractedData.value.amount <= 0) {
    errorMessage.value = 'Please enter a valid amount'
    return
  }
  
  if (!extractedData.value.description.trim()) {
    errorMessage.value = 'Please enter a description'
    return
  }
  
  emit('transactionAdded', extractedData.value)
  
  // Reset form
  selectedFile.value = null
  imagePreview.value = null
  extractedData.value = null
  clearMessages()
  successMessage.value = 'Transaction added successfully!'
  
  // Reset file input
  if (fileInput.value) {
    fileInput.value.value = ''
  }
}

// OCR processing functions
const tryOCRApproaches = async (file: File): Promise<string | null> => {
  const approaches = [
    { name: 'Tesseract.js Latvian + English', func: () => tryTesseractLatvian(file) },
    { name: 'Tesseract.js Enhanced for Receipts', func: () => tryTesseractReceiptMode(file) },
    { name: 'Tesseract.js Basic OCR', func: () => tryBasicTesseract(file) }
  ]
  
  for (const approach of approaches) {
    try {
      processingStatus.value = `Trying ${approach.name}...`
      const result = await approach.func()
      if (result && result.trim()) {
        console.log(`${approach.name} succeeded`)
        return result
      }
    } catch (error) {
      console.log(`${approach.name} failed:`, error)
    }
  }
  
  return null
}

// Primary OCR method using Tesseract.js with Latvian support
const tryTesseractLatvian = async (file: File): Promise<string | null> => {
  try {
    const worker = await createWorker(['lav', 'eng'])
    
    await worker.setParameters({
      tessedit_char_whitelist: '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyzĀāĒēĪīŌōŪūĶķĢģĻļŅņŠšŽžČč.,€$-:/ ',
      tessedit_pageseg_mode: '6',
      preserve_interword_spaces: '1'
    })
    
    const { data: { text } } = await worker.recognize(file)
    await worker.terminate()
    
    return text
  } catch (error) {
    console.error('Tesseract.js Latvian error:', error)
    throw error
  }
}

// Enhanced receipt-specific Tesseract configuration
const tryTesseractReceiptMode = async (file: File): Promise<string | null> => {
  try {
    const worker = await createWorker(['lav', 'eng'])
    
    await worker.setParameters({
      tessedit_char_whitelist: '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyzĀāĒēĪīŌōŪūĶķĢģĻļŅņŠšŽžČč.,€$-:/ *',
      tessedit_pageseg_mode: '3',
      preserve_interword_spaces: '1',
      user_defined_dpi: '300'
    })
    
    const { data: { text } } = await worker.recognize(file)
    await worker.terminate()
    
    return text
  } catch (error) {
    console.error('Tesseract.js Receipt mode error:', error)
    throw error
  }
}

// Basic Tesseract fallback
const tryBasicTesseract = async (file: File): Promise<string | null> => {
  try {
    const worker = await createWorker('eng')
    const { data: { text } } = await worker.recognize(file)
    await worker.terminate()
    return text
  } catch (error) {
    console.error('Basic Tesseract error:', error)
    throw error
  }
}

// Enhanced parsing specifically for Latvian receipts
// Enhanced parsing specifically for Latvian receipts
const parseReceiptText = (text: string): ExtractedData => {
  console.log('Parsing Latvian receipt text:', text)
  
  // Clean and normalize text
  const cleanText = text
    .replace(/\s+/g, ' ')
    .replace(/[|]/g, 'I')
    .replace(/[°]/g, '0')
    .replace(/[òó]/g, 'o')
    .replace(/[àá]/g, 'a')
    .trim()
  
  // Enhanced amount detection patterns for Latvian receipts - ALL WITH GLOBAL FLAG
  const amountPatterns = [
    /KOPĀ\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /Kopā\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /kopā\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /SUMMA\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /Summa\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /APMAKSAI\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /Apmaksai\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /KOPSUMMA\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /Kopsumma\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /MAKSĀJUMS\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /Maksājums\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /TOTAL\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /Total\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /AMOUNT\s*:?\s*€?\s*(\d+[.,]\d{2})/gi,
    /€\s*(\d+[.,]\d{2})/g,
    /(\d+[.,]\d{2})\s*€/g,
    /(\d+[.,]\d{2})\s*EUR/gi,
    /[=\-]+\s*€?\s*(\d+[.,]\d{2})\s*€?\s*$/gm,
    /(\d+[.,]\d{2})/g
  ]
  
  // Enhanced date patterns for Latvian format
  const datePatterns = [
    /(\d{1,2})[.\-/](\d{1,2})[.\-/](\d{4})/,
    /(\d{4})[.\-/](\d{1,2})[.\-/](\d{1,2})/,
    /(\d{1,2}):(\d{2}):(\d{2})\s+(\d{1,2})[.\-/](\d{1,2})[.\-/](\d{2,4})/,
    /(\d{1,2})[.\-/](\d{1,2})[.\-/](\d{2})/
  ]
  
  let amount = 0
  let description = 'Čeka pirkums'
  let category = 'Cits'
  let date = new Date().toISOString().split('T')[0]
  
  // Extract amount - more robust approach
  const foundAmounts: number[] = []
  
  // First try to find specific total keywords - these are most reliable
  const totalKeywords = [
    /kopā\s+apmaksai\s*:?\s*(\d+[.,]\d{2})/gi,
    /kopā\s*apmaksai\s*:?\s*(\d+[.,]\d{2})/gi,
    /kopā\s*:?\s*(\d+[.,]\d{2})/gi,
    /apmaksai\s*:?\s*(\d+[.,]\d{2})/gi,
    /total\s*:?\s*(\d+[.,]\d{2})/gi,
    /summa\s*:?\s*(\d+[.,]\d{2})/gi
  ]
  
  for (const pattern of totalKeywords) {
    const matches = [...text.matchAll(pattern)]
    for (const match of matches) {
      if (match[1]) {
        const cleanAmount = match[1].replace(/[€EUR\s]/g, '').replace(',', '.')
        const parsedAmount = parseFloat(cleanAmount)
        if (!isNaN(parsedAmount) && parsedAmount > 0.01 && parsedAmount < 10000) {
          foundAmounts.push(parsedAmount)
        }
      }
    }
  }
  
  // If no specific total found, try more general patterns but be more selective
  if (foundAmounts.length === 0) {
    // Look for amounts that are likely totals (avoid item codes like A-21,002)
    const betterAmountPatterns = [
      /€\s*(\d+[.,]\d{2})/g,
      /(\d+[.,]\d{2})\s*€/g,
      /(\d+[.,]\d{2})\s*EUR/gi,
      // Only match standalone amounts, not parts of codes
      /(?:^|\s)(\d+[.,]\d{2})(?:\s|$)/gm,
      // Match amounts at end of lines (often totals)
      /(\d+[.,]\d{2})\s*$/gm
    ]
    
    for (const pattern of betterAmountPatterns) {
      const matches = [...text.matchAll(pattern)]
      for (const match of matches) {
        const amountStr = match[1] || match[0]
        if (amountStr) {
          const cleanAmount = amountStr.replace(/[€EUR\s]/g, '').replace(',', '.')
          const parsedAmount = parseFloat(cleanAmount)
          // Be more restrictive - avoid very small amounts and very large amounts
          if (!isNaN(parsedAmount) && parsedAmount > 0.50 && parsedAmount < 1000) {
            foundAmounts.push(parsedAmount)
          }
        }
      }
    }
  }
  
  if (foundAmounts.length > 0) {
    // Remove duplicates and sort
    const uniqueAmounts = [...new Set(foundAmounts)].sort((a, b) => b - a)
    console.log('Found amounts:', uniqueAmounts)
    
    // For receipts, usually the largest reasonable amount is the total
    // But avoid amounts that seem too large compared to others
    amount = uniqueAmounts[0]
    
    // If the largest amount is significantly larger than the second largest,
    // it might be a false positive - use the second largest instead
    if (uniqueAmounts.length > 1 && uniqueAmounts[0] > uniqueAmounts[1] * 2) {
      amount = uniqueAmounts[1]
    }
  }
  
  // Extract date
  for (const pattern of datePatterns) {
    const match = text.match(pattern)
    if (match) {
      let year: number, month: number, day: number
      
      if (match.length >= 4) {
        if (match[3] && match[3].length === 4) {
          day = parseInt(match[1])
          month = parseInt(match[2])
          year = parseInt(match[3])
        } else if (match[1] && match[1].length === 4) {
          year = parseInt(match[1])
          month = parseInt(match[2])
          day = parseInt(match[3])
        } else if (match.length >= 7) {
          day = parseInt(match[4])
          month = parseInt(match[5])
          year = parseInt(match[6])
          if (year < 100) year += 2000
        } else if (match[3] && match[3].length === 2) {
          day = parseInt(match[1])
          month = parseInt(match[2])
          year = parseInt(match[3]) + 2000
        }
      }
      
      if (year! && month! && day! && year >= 2020 && year <= 2030 && month >= 1 && month <= 12 && day >= 1 && day <= 31) {
        date = `${year}-${month.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`
        break
      }
    }
  }
  
  // Enhanced store/category detection for Latvian stores
  const storePatterns = [
    { pattern: /maxima/i, category: 'Pārtika', description: 'Maxima pārtikas preču pirkums' },
    { pattern: /rimi/i, category: 'Pārtika', description: 'Rimi pārtikas preču pirkums' },
    { pattern: /iki/i, category: 'Pārtika', description: 'IKI pārtikas preču pirkums' },
    { pattern: /lidl/i, category: 'Pārtika', description: 'Lidl pārtikas preču pirkums' },
    { pattern: /elvi/i, category: 'Pārtika', description: 'Elvi pārtikas preču pirkums' },
    { pattern: /top!/i, category: 'Pārtika', description: 'Top! pārtikas preču pirkums' },
    { pattern: /mego/i, category: 'Iepirkšanās', description: 'Mego iepirkšanās' },
    { pattern: /spice/i, category: 'Iepirkšanās', description: 'Spice iepirkšanās' },
    { pattern: /aptieka|pharmacy/i, category: 'Veselība', description: 'Aptieka pirkums' },
    { pattern: /benu|mēness aptieka/i, category: 'Veselība', description: 'Aptieka pirkums' },
    { pattern: /neste|circle k|virši|gotika|degviela/i, category: 'Transports', description: 'Degvielas pirkums' },
    { pattern: /restorāns|restaurant|kafejnīca|cafe/i, category: 'Pārtika', description: 'Restorāns/Kafejnīca' },
    { pattern: /mcdonald|burger|kfc|hesburger/i, category: 'Pārtika', description: 'Ātrā ēdināšana' },
    { pattern: /stāvvieta|parking/i, category: 'Transports', description: 'Stāvvietas maksa' },
    { pattern: /biļete|ticket/i, category: 'Transports', description: 'Transporta biļete' },
    { pattern: /kino|cinema/i, category: 'Izklaide', description: 'Kino biļete' },
    { pattern: /teātris|theatre/i, category: 'Izklaide', description: 'Teātra biļete' },
    { pattern: /pārtikas|veikals|grocery/i, category: 'Pārtika', description: 'Pārtikas preču pirkums' },
    { pattern: /iepirkšanās|shopping/i, category: 'Iepirkšanās', description: 'Iepirkšanās' }
  ]
  
  for (const store of storePatterns) {
    if (store.pattern.test(text)) {
      category = store.category
      description = store.description
      break
    }
  }
  
  console.log('Parsed Latvian receipt result:', { amount, description, category, date })
  
  return {
    amount: amount || 0,
    description,
    category,
    date
  }
}
</script>