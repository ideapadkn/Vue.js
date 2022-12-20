<template>
  <div class="app">
    <h2>Create post</h2>
    <input type="text" v-model.trim="modificatorValue">
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
    />
  </div>
</template>


<script>
import PostForm from './components/PostForm.vue';
import PostList from './components/PostList.vue';

export default {
  components: {
    PostForm,
    PostList
  },
  data() {
    return {
      posts: [
        {id: 1, title: 'JavaScript', body: 'Desc post'},
        {id: 2, title: 'JavaScript 2', body: 'Desc post 2'},
        {id: 3, title: 'JavaScript 3', body: 'Desc post 3'},
        {id: 4, title: 'JavaScript 4', body: 'Desc post 4'},
      ],
      dialogVisiable: false,
      modificatorValue: '',
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
  },
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