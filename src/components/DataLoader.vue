<template>
  <main>
    <div v-bind:style="{maxWidth: '700px', margin: 'auto'}">
      <div class="grid" v-if="isLoaded==true">
        <div class="wrap" v-for="value in dataContent" :key="value.id">
          <div class="infoHeader">
            <div
              v-bind:style="{background:'url('+value.user.profile_image.small+') no-repeat', width:'30px', height: '30px', borderRadius: '50%', display: 'inline-block', position: 'absolute', left: '10px', top:'10px'}"
            ></div>
            <div class="info">
              <a :href="value.user.links.html" target="_blank">
                <p v-bind:style="{margin:'0px'}">{{value.user.name}}</p>
              </a>
              <a
                v-if="value.user.twitter_username!=null"
                :href="'http://twitter.com/' + value.user.twitter_username"
                target="_blank"
              >
                <p
                  v-bind:style="{ color: 'gray',fontWeight:'normal', margin:'0px'}"
                >@{{value.user.twitter_username}}</p>
              </a>
            </div>
          </div>
          <div class="picture" v-bind:style="{background:'url('+value.urls.regular+') no-repeat'}"></div>
          <div class="like">{{value.likes}}♥</div>
        </div>
      </div>
      <div v-else class="lds-dual-ring"></div>
    </div>
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
  </main>
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
  mounted: async function load() {
    await unsplash.photos
      .listPhotos(this.currentPage, this.perPage , "latest")
      .then(toJson)
      .then(json => {
        this.dataContent = json;
        this.isLoaded = true;
        this.currentPage;
      });
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
.lds-dual-ring {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #fff;
  border-color: rgb(0, 4, 255) transparent rgb(0, 4, 255) transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

h3 {
  margin: 40px 0 0;
}
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

.infoHeader {
  height: 50px;
  position: relative;
  width: 320px;
  margin: auto;
  @media (min-width: 480px) {
    width: 440px;
  }
  @media (min-width: 768px) {
    width: 320px;
  }
}

.info {
  display: inline-block;
  font-weight: bold;
  font-size: 80%;
  position: absolute;
  left: 50px;
  top: 10px;
  text-align: left;
}

.picture {
  width: 320px;
  height: 230px;
  background-position: 50%;
  background-size: cover !important;
  margin: auto;
  @media (min-width: 480px) {
    width: 440px;
    height: 306px;
  }

  @media (min-width: 768px) {
    width: 320px;
    height: 230px;
  }
}

.like {
  text-align: right;
  width: 320px;
  margin: auto;
  font-size: 90%;
  color: gray;
  padding: 10px 10px 0px 0px;
  @media (min-width: 480px) {
    width: 400px;
  }
  @media (min-width: 768px) {
    width: 320px;
  }
}

.wrap {
  height: 304px;
  padding: 0px 10px 0px 10px;
  @media (min-width: 480px) {
    height: 380px;
  }
  @media (min-width: 768px) {
    height: 312px;
  }
}

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

.grid {
  display: grid;
  grid-template-columns: 1fr;
  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr;
  }
}
</style>