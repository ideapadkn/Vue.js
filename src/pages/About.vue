<template>
  <div class="main">
    <h1 class="title">This is About page</h1>
    <router-link to="/users">Users</router-link>
    <router-link 
      to="/"
      style="margin-left: 10px;"
    >
      Main
    </router-link>
  </div>
</template>

<script>
  export default {
    
  }
</script>

<style scoped>
.main {
  background-color: antiquewhite;
}
.title {
  color: teal;
  font-size: 24px;
}
</style>