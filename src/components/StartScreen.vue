<!-- StartScreen.vue -->
<template>
  <div class="start-screen">
    <h1 class="title">QuizBlitz</h1>
    <p class="tagline">Test your knowledge. Beat the clock. Become a legend.</p>
    <RouterLink to="/play"><button class="play-btn">Play</button></RouterLink>
  </div>
</template>

<script>
export default {
  name: 'StartScreen',
};
</script>

<style scoped>
.start-screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  padding: 2rem;
}

.title {
  font-size: 4rem;
  font-weight: 900;
  color: #fff;
  letter-spacing: -1px;
  margin: 0 0 0.5rem;
  text-shadow: 0 0 30px rgba(99, 179, 237, 0.6);
  background: linear-gradient(90deg, #63b3ed, #b794f4);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.tagline {
  font-size: 1.15rem;
  color: #a0aec0;
  margin: 0 0 2.5rem;
  max-width: 340px;
  line-height: 1.6;
}

.play-btn {
  padding: 0.9rem 3rem;
  font-size: 1.2rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: #fff;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(102, 126, 234, 0.45);
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}

.play-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 32px rgba(102, 126, 234, 0.6);
}

.play-btn:active {
  transform: translateY(0);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}
</style>