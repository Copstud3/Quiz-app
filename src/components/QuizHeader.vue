<template>
    <div class="p-4 bg-purple-600">
      <div class="flex justify-between items-center text-white">
        <quiz-timer :timeLeft="timeLeft" />
        <div class="flex items-center gap-2">
          <div class="text-sm">Goal: {{ totalPoints }} points</div>
          <div class="text-sm">Current Points: {{ currentPoints }}</div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import QuizTimer from './QuizTimer.vue'
  
  export default {
    components: {
      QuizTimer
    },
    props: {
      timeLeft: Number,
      totalPoints: Number,
      currentPoints: Number
    }
  }
  </script>