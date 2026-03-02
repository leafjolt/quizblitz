<template>
  <div id="app">
    <StartScreen @start="() => {}" />
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'
export default {
  components: { StartScreen }
}
</script>