<template>
  <div id="app">
    <DataLoader 
    :currentPage = "currentPage"
    :isLoaded = "isLoaded"
    @changed="changed"
    />
    <Pagination 
    @changed="changed" 
    :currentPage = "currentPage"
    :total = "total"
    :perPage = "perPage"
    :rangeNumber = "rangeNumber"
    />
  </div>
</template>

<script>
import DataLoader from './components/DataLoader.vue';
import Pagination from './components/Pagination.vue';
export default {
  name: 'App',
  components: {
    DataLoader,
    Pagination,
  },
  data(){
    return{
      currentPage: 1,
      total: 17,
      perPage: 10,
      rangeNumber: 2,
      isLoaded: false,
    }
  },
  methods:{
    changed(currentPage){
      this.currentPage = currentPage
    }
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
