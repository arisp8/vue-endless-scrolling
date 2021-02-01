<template>
  <div id="app">
    <div class="search-box">
      <input type="text" v-model="searchQuery"/>
    </div>
    <EndlessList :searchQuery="searchQuery" />
  </div>
</template>

<script>
import EndlessList from './components/EndlessList.vue'

export default {
  name: 'App',
  components: {
    EndlessList
  },
  data() {
    return {
      searchQuery: ''
    }
  }
}
</script>

<style>
.search-box {
  padding: 0 10px 0 10px;
  position: relative;
  box-sizing: border-box;
}
.search-box input[type=text] {
  width: 100%;
  border-radius: 4px;
  border: solid 1px #ccc;
  margin: 0 auto;
  display: block;
  padding: 4px;
  font-size: 14px;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  max-width: 500px;
  border-left: solid 1px #ccc;
  border-right: solid 1px #ccc;
  margin: 0 auto;
  box-sizing: border-box;
}
</style>
