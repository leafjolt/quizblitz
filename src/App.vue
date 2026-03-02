<template>
  <div id="app">
    <!-- <StartScreen @start="() => {}" /> -->
    <QuestionCard :question="exampleQuestion" @answer="handleAnswer" />
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'
import QuestionCard from './components/QuestionCard.vue';
const exampleQuestion = {
  question: "Which planet is closest to the Sun?",
  answers: ["Earth", "Venus", "Mercury", "Mars"],
  correct: 2
}
export default {
  components: { StartScreen, QuestionCard },
  data() {
    return {
      exampleQuestion: exampleQuestion
    }
  }
}
</script>