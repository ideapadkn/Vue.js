<template>
  <div>
    <h2>Create post</h2>
    <my-input 
      v-model="searchQuery"
      placeholder="Search..."
    />
    <div class="app__btns">
      <my-button
        @click="showDialog"
      >
        Create post
      </my-button>
      <my-select 
        v-model="selectedSort"
        :options="sortOptions"
      />
    </div>
    <my-dialog 
      v-model:show="dialogVisiable"
    >
      <post-form
        @create="createPost"  
      />
    </my-dialog>
  
    <post-list 
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>Loading...</div>
    <div 
      ref="observer"
      class="observer"
    >

    </div>
    <!-- <div class="page__wrapper">
      <div 
        v-for="pageNumber in totalPages" 
        :key="pageNumber"
        class="page" 
        :class="{
          'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
  </div>
</template>


<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
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
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'kn'},
        {value: 'body', name: 'js '},
      ]
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
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    // },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get(`https://jsonplaceholder.typicode.com/posts`, {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert('Error')
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get(`https://jsonplaceholder.typicode.com/posts`, {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('Error')
      }
    },
  },
  mounted() {
    this.fetchPosts();
    let options = {
      rootMargin: '0px',
      threshold: 1.0
    }
    let callback = (entries, observer) => {
      if(entries[0].isIntersecting && this.page < this.totalPages) {
        this.loadMorePosts();
      }
    };
    let observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer);
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]);
      })
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
    }
  },
  watch: {
    // selectedSort(newValue) {
    //   this.posts.sort((post1, post2) => {
    //     return post1[newValue]?.localeCompare(post2[newValue]);
    //   })
    // },

    // page() {
    //   this.fetchPosts();
    // }
  }
}
</script>


<style>
.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
.page__wrapper {
  display: flex;
  margin-top: 15px;
}
.page {
  border: 1px solid #000;
  padding: 10px;
}
.current-page {
  border: 2px solid teal;
}
.observer {
  height: 30px;
  background-color: green;
}
</style>

// single file component