<template>
    <div class="container">
      <div class="row justify-content-center mb-4">
        <div class="col-md-3">
          <h1 class="text-center text-primary">Список публикаций</h1>
        </div>
      </div>
       <div class="row justify-content-center mb-4">
        <div class="col-md-3">
          <div class="input-group">
            <div class="input-group-prepend">
              <span class="input-group-text icon-container">
                <font-awesome-icon icon="search"></font-awesome-icon>
              </span>
            </div>
            <input type="text" class="form-control input-field" v-model="searchQuery" placeholder="Введите имя автора">
          </div>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-md-10">
          <div class="row">
            <div v-for="post in filteredPosts" :key="post.id" class="col-lg-4 col-md-6 col-sm-6 mb-4">
              <div class="post">
                <h2 class="text-primary">{{ post.title }}</h2>
                <p>{{ post.body }}</p>
                <p class="text-secondary">{{ getAuthorName(post.userId) }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  
  import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
  
  export default {
    data() {
      return {
        posts: [],
        authors: [],
        searchQuery: ''
      };
    },
    created() {
      this.fetchData();
    },
    methods: {
      async fetchData() {
        try {
          const postsResponse = await fetch('https://jsonplaceholder.typicode.com/posts');
          const usersResponse = await fetch('https://jsonplaceholder.typicode.com/users');
          this.posts = await postsResponse.json();
          this.authors = await usersResponse.json();
        } catch (error) {
          console.error('Error fetching data:', error);
        }
      },
      getAuthorName(userId) {
        const author = this.authors.find(author => author.id === userId);
        return author ? author.name : 'Unknown';
      }
    },
    computed: {
      filteredPosts() {
        return this.posts.filter(post => {
          const author = this.authors.find(author => author.id === post.userId);
          if (!author) return false;
          const authorName = author.name.toLowerCase();
          const query = this.searchQuery.toLowerCase();
          return authorName.includes(query);
        });
      }
    },
    components: {
      FontAwesomeIcon
    },
   
  };
  </script>
  
  <style lang="scss">
  
  @import "~bootstrap/scss/bootstrap";
  
  body{
    background-color: #eaeff3;
    padding-top: 2%;
  }
  
  .post {
    background-color: #fefefe;
    padding: 20px;
    border: 1px solid #dee2e6;
    border-radius: 5px;
    margin-bottom: 20px;
  }
  
  .text-primary {
    color: #007bff !important;
    font-size: x-large;
  }
  
  .text-secondary {
    color: #6c757d !important;
    font-size: large;
  }
  
  .form-control {
    width: 100%;
    font-size: large;
  }
  
  .icon-container {
    background-color: #fefefe;
    color: #aeb4b8;
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
    border-end-end-radius: 0px;
    border-top-right-radius: 0px;
    padding: 12px;
    font-size: 1rem;
  }
  </style>
  