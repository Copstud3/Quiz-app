<template>
    <div class="text-center p-8 flex flex-col justify-center items-center shadow-lg">
      <h2 class="text-2xl font-bold mb-4">Quiz Completed!</h2>
      <div class="mb-6">
        <p class="text-lg mb-2">Your Score: {{ score }} / {{ totalPoints }}</p>
        <p class="text-gray-600">
          {{ timeExpired ? 'Time has expired!' : 'You completed all questions!' }}
        </p>
      </div>
      <div class="space-y-4">
        <p class="font-medium">Completed in: {{ formatTime }}</p>
        <button
          @click="$emit('restart')"
          class="bg-purple-600 text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition-colors"
        >
          Try Again
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      score: Number,
      totalPoints: Number,
      timeExpired: Boolean,
      timeTaken: Number
    },
    computed: {
      formatTime() {
        const minutes = Math.floor(this.timeTaken / 60)
        const seconds = this.timeTaken % 60
        return `${minutes}m ${seconds}s`
      }
    }
  }
  </script>