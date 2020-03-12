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

export default {
  name: "Pagination",
  props: ['total','perPage','rangeNumber'],
  data: function() {
    return {
      currentPage: 1,
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
    //   this.isLoaded = false;
      this.$emit("changed", this.currentPage);
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
    ul {
    list-style-type: none;
    padding: 0;
    }
    li {
    display: inline-block;
    margin: 0 10px;
    }
    a {
    color: black;
    text-decoration: none;
    }
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