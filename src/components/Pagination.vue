<template>
    <div class="footer">
      <ul>
        <li v-if="hasFirst()"><a href ="#" @click="changePage(1)">1</a></li>
        <li v-if="hasFirst()">...</li>
        <li v-for="page in pages" :key="page">
          <a href ="#" @click="changePage(page)" :class="{current : currentPage == page}">{{ page }}</a>
        </li>
        <li v-if="hasLast()">...</li>
        <li v-if="hasLast() "><a href ="#" @click="changePage(total)">{{total}}</a></li>
      </ul>
    </div>
</template>

<script>
import Unsplash, { toJson } from "unsplash-js";

const unsplash = new Unsplash({
  accessKey: "p4bxahnq0buGn6UMZU7RxBMi7i4zFSYEa3uyBwkys0k",
});

export default {
  name: "DataLoader",
  data: function() {
    return {
      isLoaded: false,
      dataContent: [],
      total: 17,
      perPage: 10,
      currentPage: 1,
      rangeNumber: 2,
    };
  },
  methods:{
    hasFirst: function(){
      return this.rangeStart !=1
    },
    hasLast: function(){
      return this.rangeEnd < this.total
    },
    changePage(page){
      this.currentPage = page;
      this.isLoaded = false;
      this.load();
    },
    async load() {
    await unsplash.photos
      .listPhotos(this.currentPage, this.perPage, "latest")
      .then(toJson)
      .then(json => {
        this.dataContent = json;
        this.isLoaded = true;
        this.currentPage;
      });
  },
  },
  computed:{
    pages :function(){
      let pages = []
      for (let i=this.rangeStart; i<=this.rangeEnd; i++){
        pages.push(i)
      }
      return pages
    },
    rangeStart: function(){
      let start = this.currentPage - this.rangeNumber
      return (start >0)? start: 1;
    },
    rangeEnd: function(){
      let end = this.currentPage + this.rangeNumber
      return (end <= this.total)? end: this.total
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>
.footer {
  position: fixed;
  top: 100%;
  transform: translateY(-100%);
  height: 60px;
  width: 100%;
  background: rgba(0, 0, 0, 0.835);
  li{
    color: white;
    font-size: 100%;;
  }
  a{
    color: white;
      &.current{
      font-size: 120%;
      }
  }
}
</style>