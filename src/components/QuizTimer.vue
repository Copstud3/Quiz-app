<template>
    <div class="text-sm text-white flex items-center gap-2">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
        <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-12a1 1 0 10-2 0v4a1 1 0 00.293.707l2.828 2.829a1 1 0 101.415-1.415L11 9.586V6z" clip-rule="evenodd" />
      </svg>
      <span>{{ formatTime }}</span>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      timeLeft: Number
    },
    computed: {
      formatTime() {
        const minutes = Math.floor(this.timeLeft / 60)
        const seconds = this.timeLeft % 60
        return `${minutes}:${seconds.toString().padStart(2, '0')}`
      }
    }
  }
  </script>
  