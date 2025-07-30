<template>
  <div class="quiz-container">
    <h1>Do or Don't?</h1>
    <p class="question">{{ currentQuestion.question }}</p>

    <div class="buttons">
      <button @click="answer(true)">✅</button>
      <button @click="answer(false)">❌</button>
    </div>

    <div v-if="showResult" class="result">
      <h2>Result</h2>
      <p v-for="(item, index) in answers" :key="index">
        {{ item.question }} — 
        <strong>{{ item.correct ? '✅ Correct' : '❌ Wrong' }}</strong>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const questions = [
  { question: 'Use strong passwords', answer: true },
  { question: 'Share your passwords', answer: false },
  { question: 'Click suspicious links', answer: false },
  { question: 'Update software regularly', answer: true }
]

const current = ref(0)
const answers = ref([])
const showResult = ref(false)

const currentQuestion = computed(() => questions[current.value])

function answer(userAnswer) {
  answers.value.push({
    question: currentQuestion.value.question,
    correct: userAnswer === currentQuestion.value.answer
  })

  if (current.value < questions.length - 1) {
    current.value++
  } else {
    showResult.value = true
  }
}
</script>

<style scoped>
.quiz-container {
  max-width: 600px;
  margin: 100px auto;
  text-align: center;
  font-family: sans-serif;
}

.question {
  font-size: 24px;
  margin: 20px 0;
}

.buttons button {
  font-size: 24px;
  padding: 10px 20px;
  margin: 10px;
}

.result {
  margin-top: 40px;
  text-align: left;
}
</style>
