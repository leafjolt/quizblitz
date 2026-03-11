<!-- ScoreBoard.vue -->
<template>
  <div class="scoreboard">
    <div class="card">
      <h1 class="heading">Game Over</h1>

      <div class="score-display">
        <span class="score-value">{{ score }}</span>
        <span class="score-divider">/</span>
        <span class="score-total">{{ total }}</span>
      </div>

      <p class="score-label">{{ resultMessage }}</p>

      <div class="stars">
        <span
          v-for="n in 10"
          :key="n"
          class="star"
          :class="{ filled: n <= score }"
        >★</span>
      </div>

      <button class="restart-btn" @click="handleRestart">Play Again</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ScoreBoard',

  emits: ['restart'],

  props: {
    score: {
      type: Number,
      required: true,
      validator: (value) => value >= 0 && value <= 10,
    },
    total: {
      type: Number,
      default: 10
    }
  },

  computed: {
    resultMessage() {
      if (this.score === 10) return '🏆 Perfect score! Unbelievable!';
      if (this.score >= 8)  return '🎉 Amazing — you really know your stuff!';
      if (this.score >= 6)  return '👍 Solid effort — so close to the top!';
      if (this.score >= 4)  return '😅 Not bad, but there\'s room to grow.';
      return '💪 Keep practising — you\'ve got this!';
    },
  },

  methods: {
    handleRestart() {
      this.$emit('restart');
    },
  },
};
</script>

<style scoped>
.scoreboard {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  padding: 2rem;
}

.card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(12px);
  border-radius: 1.5rem;
  padding: 3rem 2.5rem;
  text-align: center;
  max-width: 420px;
  width: 100%;
  box-shadow: 0 24px 60px rgba(0, 0, 0, 0.4);
}

.heading {
  font-size: 2.5rem;
  font-weight: 900;
  margin: 0 0 1.5rem;
  background: linear-gradient(90deg, #63b3ed, #b794f4);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.score-display {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 0.25rem;
  margin-bottom: 0.75rem;
}

.score-value {
  font-size: 5rem;
  font-weight: 900;
  color: #fff;
  line-height: 1;
}

.score-divider {
  font-size: 2.5rem;
  color: #4a5568;
  font-weight: 300;
  margin: 0 0.1rem;
}

.score-total {
  font-size: 2.5rem;
  font-weight: 700;
  color: #718096;
}

.score-label {
  font-size: 1rem;
  color: #a0aec0;
  margin: 0 0 1.5rem;
  min-height: 1.5rem;
}

.stars {
  display: flex;
  justify-content: center;
  gap: 0.2rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.star {
  font-size: 1.4rem;
  color: #2d3748;
  transition: color 0.2s ease;
}

.star.filled {
  color: #f6c90e;
  text-shadow: 0 0 8px rgba(246, 201, 14, 0.6);
}

.restart-btn {
  width: 100%;
  padding: 0.9rem;
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: 0.5px;
  color: #fff;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(102, 126, 234, 0.4);
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}

.restart-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 32px rgba(102, 126, 234, 0.6);
}

.restart-btn:active {
  transform: translateY(0);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}
</style>