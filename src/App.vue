<template>
  <div id="app">
    <WelcomeMessage msg="Exchange Server Tracking System"/>
    <MyForm2/>

  </div>
</template>

<script>
import MyForm2 from './components/MyForm2.vue'
import WelcomeMessage from './components/WelcomeMessage.vue'

export default {
  name: 'App',
  components: {
    WelcomeMessage,
    MyForm2
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
