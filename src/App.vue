<template>
  <div class="h-screen bg-gray-100">
    <!-- Show StartScreen if quiz has not started -->
    <start-screen v-if="!isStarted" @start="startQuiz" />
    <div class="max-w-md mx-auto bg-white rounded-xl rounded-t-none max-sm:h-screen shadow-lg overflow-hidden">
      
      <!-- Show Quiz if started -->
      <transition name="fade" mode="out-in">
        <quiz-complete
          v-if="isComplete"
          :score="currentPoints"
          :totalPoints="totalPoints"
          :timeExpired="timeExpired"
          :timeTaken="totalTime - timeLeft"
          @restart="restartQuiz"
        />
        <div v-else-if="isStarted">
          <quiz-header
            :timeLeft="timeLeft"
            :totalPoints="totalPoints"
            :currentPoints="currentPoints"
          />

          <div class="p-6" v-if="currentQuestion">
            <h2 class="text-xl font-semibold mb-4 text-gray-900">
              Question {{ currentQuestionIndex + 1 }}
            </h2>
            <p class="mb-6 text-gray-600">
              {{ currentQuestion.question || '' }}
            </p>

            <transition name="fade" mode="out-in">
              <div :key="currentQuestionIndex">
                <multiple-choice
                  v-if="currentQuestion.type === 'multiple-choice'"
                  :options="currentQuestion.options || []"
                  :showFeedback="showFeedback"
                  :selectedAnswer="selectedAnswer"
                  :isCorrect="isCorrect"
                  @select-option="selectOption"
                />
                
                <drag-drop
                  v-else-if="currentQuestion.type === 'drag-drop'"
                  :definition="currentQuestion.definition || ''"
                  :terms="currentQuestion.terms || []"
                  :selectedAnswer="selectedAnswer"
                  @drop="handleDrop"
                />
              </div>
            </transition>

            <transition name="fade">
              <div v-if="showFeedback" :class="[
                'mt-6 p-4 rounded-lg',
                isCorrect ? 'bg-green-50 text-green-700' : 'bg-red-50 text-red-700'
              ]">
                <p>{{ feedbackMessage }}</p>
              </div>
            </transition>

            <button
              @click="nextQuestion"
              class="mt-6 w-full bg-purple-600 hover:bg-purple-700 text-white py-3 px-4 rounded-lg transition-colors"
            >
              Continue →
            </button>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'
import QuizHeader from './components/QuizHeader.vue'
import MultipleChoice from './components/MultipleChoice.vue'
import DragDrop from './components/DragDrop.vue'
import QuizComplete from './components/QuizComplete.vue'

export default {
  components: {
    StartScreen,
    QuizHeader,
    MultipleChoice,
    DragDrop,
    QuizComplete
  },
  data() {
    return {
      isStarted: false, // Controls when quiz starts
      totalTime: 300,
      timeLeft: 300,
      timerInterval: null,
      timeExpired: false,
      isComplete: false,
      totalPoints: 100,
      currentPoints: 0,
      currentQuestionIndex: 0,
      selectedAnswer: null,
      showFeedback: false,
      isCorrect: false,
      feedbackMessage: '',
      questions: [
          {
            type: 'multiple-choice',
            question: 'What do plants need for photosynthesis?',
            options: [
              { text: 'Oxygen & Sugar', correct: false },
              { text: 'Sunlight, Water & Carbon Dioxide', correct: true },
              { text: 'Protein & Salt', correct: false },
              { text: 'Garri and Sugar', correct: false }
            ]
          },
          {
            type: 'drag-drop',
            question: 'Match the Algebraic Terms!',
            definition: 'A fixed number that does not change',
            terms: ['Constant', 'Coefficient', 'Expression', 'Equation'],
            correctAnswer: 'Constant'
          },
          {
            type: 'multiple-choice',
            question: 'Which planet is known as the Red Planet?',
            options: [
              { text: 'Venus', correct: false },
              { text: 'Mars', correct: true },
              { text: 'Jupiter', correct: false },
              { text: 'Saturn', correct: false }
            ]
          },
          {
            type: 'drag-drop',
            question: 'Match the Scientific Term!',
            definition: 'The process by which plants convert light energy into chemical energy',
            terms: ['Photosynthesis', 'Respiration', 'Osmosis', 'Diffusion'],
            correctAnswer: 'Photosynthesis'
          },
          {
            type: 'multiple-choice',
            question: 'What is the capital of Japan?',
            options: [
              { text: 'Seoul', correct: false },
              { text: 'Beijing', correct: false },
              { text: 'Tokyo', correct: true },
              { text: 'Bangkok', correct: false }
            ]
          },
          {
            type: 'drag-drop',
            question: 'Match the Literary Term!',
            definition: 'A comparison between two unlike things using "like" or "as"',
            terms: ['Simile', 'Metaphor', 'Personification', 'Alliteration'],
            correctAnswer: 'Simile'
          },
          {
            type: 'multiple-choice',
            question: 'Which element has the chemical symbol "Au"?',
            options: [
              { text: 'Silver', correct: false },
              { text: 'Copper', correct: false },
              { text: 'Gold', correct: true },
              { text: 'Iron', correct: false }
            ]
          },
          {
            type: 'drag-drop',
            question: 'Match the Mathematical Term!',
            definition: 'A number that can be divided into another number without a remainder',
            terms: ['Factor', 'Multiple', 'Prime', 'Composite'],
            correctAnswer: 'Factor'
          },
          {
            type: 'multiple-choice',
            question: 'Who painted the Mona Lisa?',
            options: [
              { text: 'Vincent van Gogh', correct: false },
              { text: 'Leonardo da Vinci', correct: true },
              { text: 'Pablo Picasso', correct: false },
              { text: 'Michelangelo', correct: false }
            ]
          },
          {
            type: 'drag-drop',
            question: 'Match the Geography Term!',
            definition: 'A large body of salt water that connects to the ocean',
            terms: ['Sea', 'Lake', 'River', 'Stream'],
            correctAnswer: 'Sea'
          }
        ]
    }
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex] || null;
    }
  },
  beforeUnmount() {
    this.stopTimer()
  },
  methods: {
    startQuiz() {
      this.isStarted = true
      this.startTimer() // Start timer only when quiz begins
    },
    startTimer() {
      this.timeLeft = this.totalTime // Reset timer
      this.timerInterval = setInterval(() => {
        if (this.timeLeft > 0) {
          this.timeLeft--
        } else {
          this.completeQuiz(true)
        }
      }, 1000)
    },
    stopTimer() {
      clearInterval(this.timerInterval)
    },
    completeQuiz(expired = false) {
      this.stopTimer()
      this.timeExpired = expired
      this.isComplete = true
    },
    restartQuiz() {
      this.isStarted = false // Go back to start screen
      this.timeLeft = this.totalTime
      this.timeExpired = false
      this.isComplete = false
      this.currentPoints = 0
      this.currentQuestionIndex = 0
      this.selectedAnswer = null
      this.showFeedback = false
    },
    nextQuestion() {
      this.showFeedback = false
      this.selectedAnswer = null
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++
      } else {
        this.completeQuiz()
      }
    },
    handleDrop(term) {
      if (this.showFeedback) return
      
      this.selectedAnswer = term
      this.showFeedback = true
      this.isCorrect = term === this.currentQuestion.correctAnswer
      
      if (this.isCorrect) {
        this.currentPoints += 10
        this.feedbackMessage = 'Perfect match!'
      } else {
        this.feedbackMessage = 'Not correct! Next question.'
      }
    },
    selectOption(option) {
      if (this.showFeedback) return
      
      this.selectedAnswer = option
      this.showFeedback = true
      this.isCorrect = option.correct
      
      if (this.isCorrect) {
        this.currentPoints += 10
        this.feedbackMessage = 'Correct! Well done!'
      } else {
        this.feedbackMessage = 'Incorrect! Next question!'
      }
    }
  }
}
</script>
