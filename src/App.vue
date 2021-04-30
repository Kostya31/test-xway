<template>
  <div id="app">
    <Influencer ></Influencer>
  </div>
</template>

<script>
import Influencer from './components/Influencer.vue'

export default {
  name: 'App',
  components: {
    Influencer
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
