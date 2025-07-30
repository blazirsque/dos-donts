<template>
  <div class="container">
    <h1>Dos and Don'ts Quiz</h1>
    <div v-if="current" class="card" :class="{ correct: answer === true, wrong: answer === false }">
      <p class="question">{{ current.q }}</p>
      <div class="buttons" v-if="answer === null">
        <button @click="reply(true)">Yes</button>
        <button @click="reply(false)">No</button>
      </div>
      <div v-else class="answer">
        <p><strong>Rule:</strong> {{ current.rule }}</p>
        <p><strong>Explanation:</strong> {{ current.reason }}</p>
        <button @click="next">Next</button>
      </div>
    </div>
    <div v-else>
      <h2>Done! Your score: {{ score }}/{{ data.length }}</h2>
      <button @click="restart">Try Again</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const data = [
  { q: "Can we approve FTDs for the clients?", rule: "NO", reason: "Exception: only for Diamond" },
  { q: "Should we provide logins to partners?", rule: "NO", reason: "Security & transparency — managers shouldn’t share credentials" },
  // ... Добавь остальные 23 вопроса аналогично
]

const shuffled = ref([...data].sort(() => Math.random() - 0.5))
const index = ref(0)
const current = ref(shuffled.value[index.value])
const answer = ref(null)
const score = ref(0)

function reply(userAnswer) {
  answer.value = userAnswer
  if ((userAnswer && current.value.rule === "YES") || (!userAnswer && current.value.rule === "NO")) {
    score.value += 1
  }
}

function next() {
  index.value += 1
  if (index.value < shuffled.value.length) {
    current.value = shuffled.value[index.value]
    answer.value = null
  } else {
    current.value = null
  }
}

function restart() {
  shuffled.value = [...data].sort(() => Math.random() - 0.5)
  index.value = 0
  current.value = shuffled.value[0]
  answer.value = null
  score.value = 0
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
  padding: 2rem;
  font-family: sans-serif;
}
.card {
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  transition: 0.3s;
}
.correct {
  background-color: #d4edda;
}
.wrong {
  background-color: #f8d7da;
}
.question {
  font-size: 1.2rem;
  margin-bottom: 1rem;
}
.buttons button, .answer button {
  margin: 0.5rem;
  padding: 0.5rem 1rem;
}
</style>