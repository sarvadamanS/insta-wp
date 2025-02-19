<template>
  <!-- Toggle dark mode via 'dark' class -->
  <div :class="isDarkMode ? 'dark' : ''" >
    <div class="sm:flex block transition-colors bg-gray-100 dark:bg-gray-900 dark:text-white">
      <!-- LEFT SIDEBAR -->
      <LogSidebar
        :logFiles="logFiles"
        :isDarkMode="isDarkMode"
        @toggle-dark-mode="toggleDarkMode"
        class="sm:max-h-screen"
      />

      <!-- MAIN CONTENT -->
      
      <div class="flex-1 flex flex-col p-4">
        <!-- Header: Title + Search -->
        <header class="flex items-center gap-4 mb-4">
      <!-- Counters (left) -->
      <LogCounters
        :debugCount="2595"
        :infoCount="2491"
        :warningCount="2442"
        :errorCount="2475"
      />

      <!-- Search + Icons (right) -->
      <div class="sm:flex items-center ml-auto space-x-2">
        <!-- Search input -->
        <input
          v-model="searchTerm"
          @input="handleSearch"
          type="text"
          placeholder="Search logs..."
          class="border  border-gray-300 dark:border-gray-700 rounded px-2 py-1 text-sm bg-white text-gray-600 dark:bg-gray-800 dark:text-white"
        />

        <!-- Settings button -->
        <button
          class="p-1  text-gray-600 dark:text-gray-300 hover:text-gray-800 dark:hover:text-gray-100"
          @click="openSettings"
        >
          <Cog8ToothIcon class="w-5 h-5" />
        </button>

        <!-- Dark mode toggle button -->
        <button
          class="p-1 text-gray-600 dark:text-gray-300 hover:text-gray-800 dark:hover:text-gray-100"
          @click="toggleDarkMode"
        >
          <MoonIcon class="w-5 h-5" />
        </button>
      </div>
    </header>


        <!-- Log Table -->
        <LogTable
          :logs="pagedLogs"         
          :sortField="sortField"
          :sortDir="sortDir"
          @sort-by="sortBy"       
    @time-sort-changed="onTimeSortChanged"
    @page-size-changed="onPageSizeChanged"
        />

        <!-- Pagination -->
        <div class="flex items-center justify-center mt-4">
          <!-- <div class="text-sm text-gray-600 dark:text-gray-300">
            Showing {{ startIndex + 1 }} - {{ endIndex }} of {{ filteredLogs.length }}
          </div> -->
          <div class="space-x-1">
            <button
              class="px-3 py-1 rounded text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700"
              :disabled="currentPage === 1"
              @click="prevPage"
            >
             <ArrowLongLeftIcon class="w-4 h-4"/>
            </button>
            <button
              v-for="p in totalPages"
              :key="p"
              class="px-3 py-1 "
              :class="{
                'border-t-emerald-400 border-t-2 text-emerald-400': p === currentPage,
                'hover:bg-gray-200  text-gray-600 dark:text-gray-300 dark:hover:bg-gray-700': p !== currentPage
              }"
              @click="goToPage(p)"
            >
              {{ p }}
            </button>
            <button
              class="px-3 py-1 rounded text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700"
              :disabled="currentPage === totalPages"
              @click="nextPage"
            >
            <ArrowLongRightIcon class="w-4 h-4"/>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LogSidebar from './LogSidebar.vue'
import LogTable from './LogTable.vue'
import LogCounters from './LogCounters.vue'
import { Cog8ToothIcon, MoonIcon ,ArrowLongRightIcon,ArrowLongLeftIcon} from '@heroicons/vue/24/solid'

export default {
  name: 'LogViewer',
  components: { LogSidebar, LogTable,LogCounters , Cog8ToothIcon,
    MoonIcon,ArrowLongRightIcon,ArrowLongLeftIcon},
  data() {
    return {
      /* Dark mode toggle */
      isDarkMode: false,

      /* Search, sorting, pagination states */
      searchTerm: '',
      sortField: '',
      sortDir: null, // 'asc' or 'desc'
      currentPage: 1,
      pageSize: 12,

      /* Example list of log files (sidebar) */
      logFiles: [
        { name: 'laravel.log', size: '1.2MB' },
        { name: 'system.log', size: '3.4MB' },
        { name: 'app.log', size: '500kB' },
        { name: 'nginx.log', size: '2.2MB' },
        { name: 'db.log', size: '4.1MB' },
        { name: 'laravel.log', size: '1.2MB' },
        { name: 'system.log', size: '3.4MB' },
        { name: 'app.log', size: '500kB' },
        { name: 'nginx.log', size: '2.2MB' },
        { name: 'db.log', size: '4.1MB' },{ name: 'laravel.log', size: '1.2MB' },
        { name: 'system.log', size: '3.4MB' },
        { name: 'app.log', size: '500kB' },
        { name: 'nginx.log', size: '2.2MB' },
        { name: 'db.log', size: '4.1MB' },{ name: 'laravel.log', size: '1.2MB' },
        { name: 'system.log', size: '3.4MB' },
        { name: 'app.log', size: '500kB' },
        { name: 'nginx.log', size: '2.2MB' },
        { name: 'db.log', size: '4.1MB' },{ name: 'laravel.log', size: '1.2MB' },
        { name: 'system.log', size: '3.4MB' },
        { name: 'app.log', size: '500kB' },
        { name: 'nginx.log', size: '2.2MB' },
        { name: 'db.log', size: '4.1MB' },
      ],

      logs: [
      { level: 'Debug',   time: '09:00:12', env: 'local',       description: 'Debug message #1' },
      { level: 'Info',    time: '09:05:33', env: 'production',  description: 'Info message #2' },
      { level: 'Warning', time: '09:10:45', env: 'staging',     description: 'Warning message #3' },
      { level: 'Error',   time: '09:20:12', env: 'local',       description: 'Error message #4' },
      { level: 'Debug',   time: '09:22:56', env: 'production',  description: 'Debug message #5' },
      { level: 'Info',    time: '09:30:10', env: 'production',  description: 'Info message #6' },
      { level: 'Warning', time: '09:35:27', env: 'staging',     description: 'Warning message #7' },
      { level: 'Error',   time: '09:40:55', env: 'local',       description: 'Error message #8' },
      { level: 'Debug',   time: '09:45:00', env: 'local',       description: 'Debug message #9' },
      { level: 'Info',    time: '09:50:33', env: 'production',  description: 'Info message #10' },

      { level: 'Warning', time: '09:55:45', env: 'staging',     description: 'Warning message #11' },
      { level: 'Error',   time: '10:00:12', env: 'local',       description: 'Error message #12' },
      { level: 'Debug',   time: '10:05:12', env: 'production',  description: 'Debug message #13' },
      { level: 'Info',    time: '10:10:33', env: 'production',  description: 'Info message #14' },
      { level: 'Warning', time: '10:15:45', env: 'staging',     description: 'Warning message #15' },
      { level: 'Error',   time: '10:20:12', env: 'local',       description: 'Error message #16' },
      { level: 'Debug',   time: '10:22:56', env: 'production',  description: 'Debug message #17' },
      { level: 'Info',    time: '10:30:21', env: 'production',  description: 'Info message #18' },
      { level: 'Warning', time: '10:35:12', env: 'staging',     description: 'Warning message #19' },
      { level: 'Error',   time: '10:40:55', env: 'local',       description: 'Error message #20' },

      {
         level: 'Debug',   time: '10:45:00', env: 'local',       description: 'Debug message #21' },
      { level: 'Info',    time: '10:50:33', env: 'production',  description: 'Info message #22' },
      { level: 'Warning', time: '10:55:45', env: 'staging',     description: 'Warning message #23' },
      { level: 'Error',   time: '11:00:12', env: 'local',       description: 'Error message #24' },
      { level: 'Debug',   time: '11:05:12', env: 'production',  description: 'Debug message #25' },
      { level: 'Info',    time: '11:10:33', env: 'production',  description: 'Info message #26' },
      { level: 'Warning', time: '11:15:45', env: 'staging',     description: 'Warning message #27' },
      { level: 'Error',   time: '11:20:12', env: 'local',       description: 'Error message #28' },
      { level: 'Debug',   time: '11:22:56', env: 'production',  description: 'Debug message #29' },
      { level: 'Info',    time: '11:30:21', env: 'production',  description: 'Info message #30' },
      {
         level: 'Debug',   time: '10:45:00', env: 'local',       description: 'Debug message #21' },
      { level: 'Info',    time: '10:50:33', env: 'production',  description: 'Info message #22' },
      { level: 'Warning', time: '10:55:45', env: 'staging',     description: 'Warning message #23' },
      { level: 'Error',   time: '11:00:12', env: 'local',       description: 'Error message #24' },
      { level: 'Debug',   time: '11:05:12', env: 'production',  description: 'Debug message #25' },
      { level: 'Info',    time: '11:10:33', env: 'production',  description: 'Info message #26' },
      { level: 'Warning', time: '11:15:45', env: 'staging',     description: 'Warning message #27' },
      { level: 'Error',   time: '11:20:12', env: 'local',       description: 'Error message #28' },
      { level: 'Debug',   time: '11:22:56', env: 'production',  description: 'Debug message #29' },
      { level: 'Info',    time: '11:30:21', env: 'production',  description: 'Info message #30' },
    ],
    }
  },
  computed: {
    /* 1) Filter logs by searchTerm */
    filteredLogs() {
      if (!this.searchTerm) return this.logs
      const term = this.searchTerm.toLowerCase()
      return this.logs.filter(log =>
        Object.values(log).some(val => String(val).toLowerCase().includes(term))
      )
    },
    /* 2) Sort logs by selected field */
    sortedLogs() {
      if (!this.sortField) return this.filteredLogs
      const sorted = [...this.filteredLogs]
      sorted.sort((a, b) => {
        const valA = a[this.sortField]
        const valB = b[this.sortField]
        if (valA < valB) return this.sortDir === 'asc' ? -1 : 1
        if (valA > valB) return this.sortDir === 'asc' ? 1 : -1
        return 0
      })
      return sorted
    },
    /* 3) Paginated logs */
    pagedLogs() {
      const start = (this.currentPage - 1) * this.pageSize
      const end = start + this.pageSize
      return this.sortedLogs.slice(start, end)
    },
    totalPages() {
      return Math.ceil(this.filteredLogs.length / this.pageSize)
    },
    startIndex() {
      return (this.currentPage - 1) * this.pageSize
    },
    endIndex() {
      const idx = this.startIndex + this.pageSize
      return idx > this.filteredLogs.length ? this.filteredLogs.length : idx
    },
  },
  methods: {
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode
    },
    handleSearch() {
      /* Reset to page 1 whenever search changes */
      this.currentPage = 1
    },
    sortBy(field) {
      if (this.sortField === field) {
        /* Toggle direction */
        this.sortDir = this.sortDir === 'asc' ? 'desc' : 'asc'
      } else {
        this.sortField = field
        this.sortDir = 'asc'
      }
      this.currentPage = 1
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--
      }
    },
    goToPage(p) {
      this.currentPage = p
    },
    onTimeSortChanged(direction) {
      // direction is 'asc' or 'desc'
      this.sortField = 'time'
      this.sortDir = direction
      // reset to page 1 or do anything else needed
      this.currentPage = 1
    },
    onPageSizeChanged(newSize) {
      this.pageSize = newSize
      this.currentPage = 1
    },
  },
}
</script>

<style scoped>
/* Parent-level styling if needed */
</style>
