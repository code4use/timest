<template>
  <div id="app">
    <TimesT />
  </div>
</template>

<script>
import TimesT from './components/TimesT.vue'

export default {
  name: 'App',
  components: {
    TimesT
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html, body {
  margin: 0px !important;
  padding: 0px !important;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-size: 24px;
  color: #2c3e50;
  margin-top: 6px;
}
</style>
