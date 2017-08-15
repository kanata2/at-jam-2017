<template>
  <div id="app">
    <table-container></table-container>
  </div>
</template>

<script>
import TableContainer from './components/TableContainer'

export default {
  name: 'app',
  components: {
    TableContainer
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
