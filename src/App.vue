<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <hello-world></hello-world>
    <test-component></test-component>
    <div>
      <p></p>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import TestComponent from './components/TestComponent.vue'

export default {
  name: 'App',
  components: {
    'hello-world': HelloWorld,
    'test-component': TestComponent,
  },
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
