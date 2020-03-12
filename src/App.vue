<template>
  <div id="app">
    <DataLoader />
  </div>
</template>

<script>
import DataLoader from './components/DataLoader.vue';
export default {
  name: 'App',
  components: {
    DataLoader,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: hsl(210, 29%, 24%);
}
</style>
