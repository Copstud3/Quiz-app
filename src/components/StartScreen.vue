<template>
    <div class="flex flex-col items-center shadow-none justify-center min-h-screen bg-gray-100">
      <div class="bg-white p-8 rounded-xl shadow-lg text-center">
        <h1 class="text-2xl font-bold text-gray-900">Welcome to the Quiz</h1>
        <p class="text-gray-600 mt-2">Test your knowledge with fun questions!</p>
        <p class="text-gray-600 mt-2">You have exactly 5 minutes to answer 10 questions. Have fun!</p>
        <button
          @click="$emit('start')"
          class="mt-6 w-full bg-purple-600 hover:bg-purple-700 text-white py-3 px-4 rounded-lg transition-colors"
        >
          Start Quiz →
        </button>
      </div>
    </div>
  </template>
  