<template>
    <div class="flex flex-wrap items-center gap-2 m-2">
      <!-- Debug -->
      <div class="inline-flex items-center px-3 py-1 text-xs font-base text-white bg-cyan-600 rounded-sm">
        <!-- Small white box around the check icon -->
        <div class="flex items-center justify-center w-3 h-3 mr-1 rounded-xs bg-white">
          <CheckIcon class="w-2.5 h-2.5 text-cyan-600" />
        </div>
        Debug: {{ debugCount }}
      </div>
  
      <!-- Info -->
      <div class="inline-flex items-center px-3 py-1 text-xs font-base text-white bg-cyan-600 rounded-sm">
        <div class="flex items-center justify-center w-3 h-3 mr-1 rounded-xs bg-white">
          <CheckIcon class="w-2.5 h-2.5 text-cyan-600" />
        </div>
        Info: {{ infoCount }}
      </div>
  
      <!-- Warning -->
      <div class="inline-flex items-center px-3 py-1 text-xs font-base text-white bg-yellow-600 rounded-sm">
        <div class="flex items-center justify-center w-3 h-3 mr-1 rounded-xs bg-white">
          <CheckIcon class="w-2.5 h-2.5 text-yellow-600" />
        </div>
        Warning: {{ warningCount }}
      </div>
  
      <!-- Error -->
      <div class="inline-flex items-center px-3 py-1 text-xs font-base text-white bg-red-500 rounded-sm">
        <div class="flex items-center justify-center w-3 h-3 mr-1 rounded-xs bg-white">
          <CheckIcon class="w-2.5 h-2.5 text-red-500" />
        </div>
        Error: {{ errorCount }}
      </div>
    </div>
  </template>
  
  <script>
  import { CheckIcon } from '@heroicons/vue/24/solid'
  
  export default {
    name: 'LogCounters',
    components: {
      CheckIcon
    },
    props: {
      debugCount: {
        type: Number,
        default: 0
      },
      infoCount: {
        type: Number,
        default: 0
      },
      warningCount: {
        type: Number,
        default: 0
      },
      errorCount: {
        type: Number,
        default: 0
      }
    }
  }
  </script>
  
  <style scoped>
  /* Additional styling if needed */
  </style>
  