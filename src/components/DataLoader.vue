<template>
  <div class="hello">
      <input v-if='isLoaded' type='submit' @click=load>
            <div v-bind:style="{height:'304px'}" v-for="value in content" :key="value.id">
              <div v-bind:style="{height:'50px', position: 'relative', width: '320px', margin: 'auto'}">
                <div v-bind:style="{background:'url('+value.user.profile_image.small+') no-repeat', width:'30px', height: '30px', borderRadius: '50%', display: 'inline-block', position: 'absolute', left: '10px', top:'10px'}"></div>
                <a :href=value.user.links.html><div class="info"><p v-bind:style="{margin:'0px'}">{{value.user.name}}</p><p v-if="value.user.twitter_username!=null" v-bind:style="{ color: 'gray',fontWeight:'normal', margin:'0px'}">@{{value.user.twitter_username}}</p></div></a>
              </div>
                <div v-bind:style="{width: '320px', height: '230px',background:'url('+value.urls.small+') no-repeat', backgroundPosition: '50%', margin: 'auto'}"></div>
                <div v-bind:style="{textAlign: 'right',width: '320px', margin:'auto', fontSize: '90%', color:'gray', padding: '10px 10px 0px 0px'}">{{value.likes}}♥</div>
            </div>
  </div>
</template>

<script>

import Unsplash, { toJson } from "unsplash-js";

const unsplash = new Unsplash({
  accessKey: "p4bxahnq0buGn6UMZU7RxBMi7i4zFSYEa3uyBwkys0k",
  // Optionally you can also configure a custom header to be sent with every request
  headers: {
    "X-Per-Page": "15",
    "X-Total": "300",
  }
});

export default {
  name: "DataLoader",
  props: {
    content: Object
  },
  data:function() {
      return{
          url: 'http://vk.com',
        //   url: {{value.user.profile_image.small}},
    isLoaded: true,
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
      }
  },
  methods:{
    beforeMount(){
      console.log('mounted');
    },
    mounted(){
            console.log('mounted');
            unsplash.photos.listPhotos(27,10, "latest")
            .then(toJson)
            .then(json => {
              console.log(json);
              this.content = json;
    });
    this.isLoaded = false;
    }
  },
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>
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
  color: #42b983;
}

.info{
  display: inline-block;
  font-weight: bold;
  font-size: 80%;
  position: absolute;
  left: 50px;
  top:10px;
  text-align: left;
}
</style>
