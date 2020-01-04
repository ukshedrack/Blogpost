<template>
  <div id="show-blogs">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="search blogs">
    <div v-for="blog in filteredBlogs" v-bind:key="blog" class="single-blog">
        <router-link v-bind:to="'/blog/' + blog.id"><h2>{{ blog.title | to-uppercase }}</h2></router-link>
        <article>{{ blog.content | snippet }}</article>
    </div>
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';

export default {
  
  data() {
    return {
        blogs: [],
        search: ''
    }
  },
  methods: {

  },
  created() {
      this.$http.get("https://my-blog-137ad.firebaseio.com/posts.json").then(function(data){
          /* eslint-disable no-console */
          return data.json();
      }).then(function(data){
          var blogsArray = [];
          for (let key in data){
              data[key].id = key;
              blogsArray.push(data[key]);
          }
          this.blogs = blogsArray;
      })
  },
  filters: {
      toUppercase(value){
          return value.toUpperCase();
      }
  },
  directives: {
      'rainbow': {
          bind(el) {
              el.style.color = "#" + Math.random().toString(16).slice(2,8);
            }
      }
  },
  mixins: [searchMixin] 
}
</script>

<style>
#show-blogs{
    max-width: 800px;
    margin: 0 auto;
}
input[type="text"], textarea{
    display: block;
    width: 100%;
    padding: 8px;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>