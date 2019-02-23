<template>
  <div id="app">
    <CryptoList/>
  </div>
</template>

<script>
import CryptoList from './components/CryptoList.vue';

export default {
  name: 'App',
  components: {
    CryptoList,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
