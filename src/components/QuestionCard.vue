<!-- QuestionCard.vue -->
<template>
  <div class="question-card">
    <p class="question-text">{{ question.question }}</p>

    <div class="answers">
      <button
        v-for="(answer, index) in question.answers"
        :key="index"
        class="answer-btn"
        :class="getAnswerClass(index)"
        :disabled="answered"
        @click="handleAnswer(index)"
      >
        <span class="answer-letter">{{ letters[index] }}</span>
        <span class="answer-text">{{ answer }}</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionCard',

  emits: ['answer'],

  props: {
    question: {
      type: Object,
      required: true,
      validator(val) {
        return (
          typeof val.question === 'string' &&
          Array.isArray(val.answers) &&
          typeof val.correct === 'number'
        );
      },
    },
  },

  data() {
    return {
      answered: false,   // locks all buttons once one is clicked
      selectedIndex: null,
      letters: ['A', 'B', 'C', 'D'],
    };
  },

  methods: {
    handleAnswer(index) {
      if (this.answered) return;

      const isCorrect = index === this.question.correct;

      this.answered = true;
      this.selectedIndex = index;

      setTimeout(() => {
        this.$emit('answer', isCorrect);
        this.reset();
      }, 1000);
    },

    getAnswerClass(index) {
      if (!this.answered) return '';

      if (index === this.question.correct) return 'correct';

      if (index === this.selectedIndex) return 'wrong';

      return '';
    },

    reset() {
      this.answered = false;
      this.selectedIndex = null;
    },
  },
};
</script>

<style scoped>
.question-card {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  width: 100%;
  max-width: 640px;
  margin: 0 auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.question-text {
  font-size: 1.25rem;
  font-weight: 600;
  color: #fff;
  line-height: 1.6;
  text-align: center;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 1rem;
  padding: 1.5rem;
  margin: 0;
}

.answers {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
}

.answer-btn {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.85rem 1rem;
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 0.75rem;
  color: #e2e8f0;
  font-size: 0.95rem;
  font-weight: 500;
  cursor: pointer;
  text-align: left;
  transition: background 0.15s ease, border-color 0.15s ease, transform 0.1s ease;
}

.answer-btn:hover:not(:disabled) {
  background: rgba(255, 255, 255, 0.12);
  border-color: rgba(255, 255, 255, 0.25);
  transform: translateY(-2px);
}

.answer-btn:disabled {
  cursor: not-allowed;
  opacity: 0.75;
}

.answer-letter {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1.75rem;
  height: 1.75rem;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  font-size: 0.8rem;
  font-weight: 700;
  flex-shrink: 0;
  color: #a0aec0;
}

.answer-text {
  line-height: 1.35;
}

/* ── Feedback states ─────────────────────────────── */

.answer-btn.correct {
  background: rgba(72, 187, 120, 0.2);
  border-color: #48bb78;
  color: #9ae6b4;
}

.answer-btn.correct .answer-letter {
  background: #48bb78;
  color: #fff;
}

.answer-btn.wrong {
  background: rgba(245, 101, 101, 0.2);
  border-color: #f56565;
  color: #feb2b2;
}

.answer-btn.wrong .answer-letter {
  background: #f56565;
  color: #fff;
}

/* ── Responsive ──────────────────────────────────── */

@media (max-width: 480px) {
  .answers {
    grid-template-columns: 1fr;
  }

  .question-text {
    font-size: 1.1rem;
  }
}
</style>