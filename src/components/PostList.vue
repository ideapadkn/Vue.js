<template>
  <div v-if="posts.length > 0">
    <h3>List Users</h3>
    <transition-group name="post-list">
      <post-item 
        v-for="post in posts"
        :post="post"
        :key="post.id"
        @remove="$emit('remove', post)"
      />
    </transition-group>
  </div>
  <h3 
    v-else
    style="color:red;"
  >
    List Users Empty
  </h3>
</template>


<script>
import PostItem from './PostItem.vue';
  export default {
    components: {
      PostItem,
    },
    props: {
      posts: {
        type: Array,  
        required: true,
      },
    },
  }
</script>


<style scoped>
.post-list-move, /* apply transition to moving elements */
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.4s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.post-list-leave-active {
  position: absolute;
}
</style>