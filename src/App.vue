<template>
  <div id="app">
    <StartScreen v-if="gameState === 'start'" @start="startGame" />
    <QuestionCard
      v-else-if="gameState === 'playing'"
      :question="questions[currentIndex]"
      @answer="handleAnswer"
    />
    <ScoreBoard
      v-else
      :score="score"
      @restart="resetGame"
    />
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'
import QuestionCard from './components/QuestionCard.vue';
import ScoreBoard from './components/ScoreBoard.vue';
const QUESTIONS = [
  {
    question: "What is the largest planet in our solar system?",
    answers: ["Saturn", "Jupiter", "Neptune", "Uranus"],
    correct: 1,
  },
  {
    question: "Which element has the atomic number 1?",
    answers: ["Helium", "Oxygen", "Carbon", "Hydrogen"],
    correct: 3,
  },
  {
    question: "What year did the Berlin Wall fall?",
    answers: ["1987", "1991", "1989", "1993"],
    correct: 2,
  },
  {
    question: "Which country has the most natural lakes?",
    answers: ["Russia", "USA", "Canada", "Finland"],
    correct: 2,
  },
  {
    question: "What is the fastest land animal?",
    answers: ["Lion", "Cheetah", "Greyhound", "Pronghorn"],
    correct: 1,
  },
  {
    question: "How many strings does a standard guitar have?",
    answers: ["4", "5", "7", "6"],
    correct: 3,
  },
  {
    question: "Which Shakespeare play features the character Ophelia?",
    answers: ["Macbeth", "Othello", "Hamlet", "King Lear"],
    correct: 2,
  },
  {
    question: "What is the smallest country in the world by area?",
    answers: ["Monaco", "San Marino", "Vatican City", "Liechtenstein"],
    correct: 2,
  },
  {
    question: "Which gas makes up approximately 78% of Earth's atmosphere?",
    answers: ["Oxygen", "Carbon Dioxide", "Argon", "Nitrogen"],
    correct: 3,
  },
  {
    question: "Who developed the theory of general relativity?",
    answers: ["Isaac Newton", "Nikola Tesla", "Albert Einstein", "Max Planck"],
    correct: 2,
  },
];
export default {
  components: { StartScreen, QuestionCard, ScoreBoard },
  data() {
    return {
      questions: QUESTIONS, // you will fill this with your 10 questions
      currentIndex: 0,
      score: 0,
      gameState: "start" // "start" | "playing" | "end"
    }
  },
  methods: {
    startGame() {
      this.gameState = "playing";
      this.currentIndex = 0;
      this.score = 0;
    },
    handleAnswer(isCorrect) {
      if (isCorrect) {
        this.score++;
      }
      this.currentIndex++;
      if (this.currentIndex === this.questions.length) {
        this.gameState = "end";
      }
    },
    resetGame() {
      this.gameState = "start";
    }
  }
}
</script>