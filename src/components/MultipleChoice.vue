<template>
    <div class="space-y-4">
      <div
        v-for="(option, index) in options"
        :key="index"
        @click="selectOption(option)"
        :class="[
          'p-4 rounded-lg border cursor-pointer transition-all transform',
          getOptionClass(option),
          { 'shake-animation': showShake && option === selectedAnswer && !isCorrect }
        ]"
      >
        <div class="flex items-center">
          <span class="mr-3">{{ ['A', 'B', 'C', 'D'][index] }}</span>
          <span>{{ option.text }}</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      options: Array,
      showFeedback: Boolean,
      selectedAnswer: Object,
      isCorrect: Boolean
    },
    data() {
      return {
        showShake: false
      }
    },
    methods: {
      selectOption(option) {
        if (this.showFeedback) return
        this.$emit('select-option', option)
        if (!option.correct) {
          this.showShake = true
          setTimeout(() => {
            this.showShake = false
          }, 500)
        }
      },
      getOptionClass(option) {
        if (!this.showFeedback) return 'border-gray-200 hover:border-purple-500'
        if (option === this.selectedAnswer) {
          return this.isCorrect 
            ? 'border-green-500 bg-green-50'
            : 'border-red-500 bg-red-50'
        }
        return 'border-gray-200'
      }
    }
  }
  </script>