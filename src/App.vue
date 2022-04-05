<template>
  <WebGLCanvas msg="Welcome to Your Vue.js App"/>
</template>

<script>
import WebGLCanvas from './components/Scene.vue'

export default {
  name: 'App',
  components: {
    WebGLCanvas
  }
}
</script>

<style>
html, 
body {
    height: 100%;
    margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}
</style>
