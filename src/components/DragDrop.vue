<template>
    <div class="space-y-6">
      <div class="p-4 rounded-lg bg-purple-50">
        <p class="text-purple-700">{{ definition }}</p>
      </div>
      
      <div class="grid grid-cols-2 gap-4">
        <div
          v-for="(term, index) in terms"
          :key="index"
          class="p-3 rounded-lg text-center cursor-move transition-transform hover:scale-105 bg-gray-800 text-white"
          draggable="true"
          @dragstart="dragStart($event, term)"
        >
          {{ term }}
        </div>
      </div>
      
      <div
        class="mt-4 p-4 border-2 border-dashed border-gray-300 rounded-lg h-20 flex items-center justify-center transition-colors"
        @dragover.prevent
        @drop="handleDrop"
      >
        <p class="text-gray-500" v-if="!selectedAnswer">
          Drag your answer here
        </p>
        <p class="text-purple-600" v-else>
          {{ selectedAnswer }}
        </p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      definition: String,
      terms: Array,
      selectedAnswer: String
    },
    methods: {
      dragStart(event, term) {
        event.dataTransfer.setData('text/plain', term)
      },
      handleDrop(event) {
        const term = event.dataTransfer.getData('text/plain')
        this.$emit('drop', term)
      }
    }
  }
  </script>
  