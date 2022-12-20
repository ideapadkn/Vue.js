<template>
  <div class="app">
    <h2>Create post</h2>
    <!-- <input type="text" v-model.trim="modificatorValue"> -->
    <my-button
    style="margin: 15px 0;"
      @click="showDialog"
    >
      Create post
    </my-button>
    <my-dialog 
      v-model:show="dialogVisiable"
    >
      <post-form
        @create="createPost"  
      />
    </my-dialog>
  
    <post-list 
      :posts="posts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>Loading...</div>
  </div>
</template>


<script>
import PostForm from './components/PostForm.vue';
import PostList from './components/PostList.vue';
import axios from 'axios';

export default {
  components: {
    PostForm,
    PostList
  },
  data() {
    return {
      posts: [],
      dialogVisiable: false,
      isPostsLoading: false,
    }
  },
  methods: {
    createPost(post) { 
      this.posts.push(post);
      this.dialogVisiable = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisiable = true;
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        setTimeout(async () => {
          const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?_limit=10`);
          this.posts = response.data;
          this.isPostsLoading = false;
        }, 1000);
      } catch (e) {
        alert('Error')
      } finally {
        // this.isPostsLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  }
}
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app {
  padding: 20px;
}
</style>

// single file component