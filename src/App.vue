<template>
  <div id="app">
    <Music />
  </div>
</template>

<script>
import Music from './components/Music.vue'

export default {
  name: 'app',
  components: {
    Music
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 80px;
}
</style>
