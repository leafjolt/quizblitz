<template>
  <div class="home-view">
    <StartScreen />
  </div>
</template>

<script>
import StartScreen from '../components/StartScreen.vue';

export default {
  name: 'HomeView',

  components: { StartScreen },
};
</script>

<style scoped>
.home-view {
  min-height: 100vh;
}
</style>