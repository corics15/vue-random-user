<template>
  <Profile />
</template>

<script>
import Profile from './components/Profile'

export default {
  name: 'App',
  components: {
    Profile
  }
}
</script>

<style>
  html, body { background-color: #eee; }
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 30px;
  }
</style>
