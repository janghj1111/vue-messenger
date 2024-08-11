<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <Messenger msg="하이~"/>
</template>

<script>
import Messenger from './components/Messenger.vue'

export default {
  name: 'App',
  components: {
    Messenger
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
