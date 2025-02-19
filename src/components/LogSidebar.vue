<template>
    <aside class="dark:bg-gray-800 p-4 flex flex-col min-w-[240px]">
      <!-- Heading -->
       <div class="flex items-center justify-start align-baseline mb-2 gap-2">

         <h2 class="font-bold  text-emerald-600">Log Viewer</h2>
         <img class="w-4 h-4" src="/github-mark.png"/>
        </div>
         <div class="flex items-center justify-start mb-2 gap-2">

          <ArrowLongLeftIcon class="w-4 h-4 text-gray-400 "/> 
      <p class="text-gray-400 text-xs">Back to Laravel</p>
      </div>
      <!-- Scrollable List Container -->
      <div class="flex-1 space-y-2 overflow-auto pr-2">
        <div
          v-for="(file, index) in logFiles"
          :key="file.name"
          @click="selectFile(index)"
          :class="[
            'flex items-center px-4 py-1 rounded cursor-pointer',
            'hover:bg-gray-100 dark:hover:bg-gray-700',
            index === activeIndex ? 'bg-emerald-50 text-emerald-600 border-emerald-400 border-1' : 'bg-white text-gray-700 dark:bg-gray-700 dark:text-gray-200'
          ]"
        >
          <!-- File Name (left) and Size + Dots (right) -->
          <div class="flex items-center justify-between w-full">
            <!-- File Name -->
            <p class="text-xs font-medium">
              {{ file.name }}
            </p>
            <!-- File Size + 3-dot button -->
            <div class="flex items-center gap-2">
              <p class="text-xs text-gray-500" :class="index === activeIndex ? 'text-emerald-500' : ''">
                {{ file.size }}
              </p>
              <button class="p-1 text-gray-400 hover:text-gray-600 dark:hover:text-gray-300">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="w-5 h-5"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 5v.01M12 12v.01M12 19v.01"
                  />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Dark Mode Toggle -->
      <button
        class="mt-4 flex items-center px-3 py-1 text-sm border rounded
               text-gray-600 dark:text-white
               hover:bg-gray-200 dark:hover:bg-gray-700"
        @click="$emit('toggle-dark-mode')"
      >
        <MoonIcon class="w-4 h-4 mr-2" />
        {{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}
      </button>
    </aside>
  </template>
  
  <script>
  import { MoonIcon ,ArrowLongLeftIcon} from '@heroicons/vue/24/solid'
  
  export default {
    name: 'LogSidebar',
    components: {
      MoonIcon,ArrowLongLeftIcon
    },
    props: {
      logFiles: {
        type: Array,
        default: () => [],
      },
      isDarkMode: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        activeIndex: 0, // By default, select the first file
      }
    },
    methods: {
      selectFile(index) {
        this.activeIndex = index
        // Optionally emit an event for the parent:
        // this.$emit('file-selected', this.logFiles[index])
      },
    },
  }
  </script>
  
  <style scoped>
 .dark img {
    content: url("/github-mark-white.png"); /* Path to your dark mode image */
  }
  </style>
  