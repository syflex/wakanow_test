<template>
  <div id="app">
    <b-container fluid>
      <img alt="Vue logo" src="./assets/frog.svg" width="100px" class="">
      <Home msg="The Frog Life Managment"/>
    </b-container>    
  </div>
</template>

<script>
import Home from './components/Index.vue'
export default {
  name: 'App',
  components: {
    Home
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
}
</style>
