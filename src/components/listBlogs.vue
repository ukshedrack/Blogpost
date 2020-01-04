<template>
  <div id="show-blogs">
    <h1>List Blogs Title</h1>
    <input type="text" v-model="search" placeholder="search blogs">
    <div v-for="blog in filteredBlogs" v-bind:key="blog" class="single-blog">
        <h2 v-rainbow>{{ blog.title | to-uppercase }}</h2>
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
      this.$http.get("http://jsonplaceholder.typicode.com/posts").then(function(data){
          /* eslint-disable no-console */
          this.blogs = data.body.slice(0,10);
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