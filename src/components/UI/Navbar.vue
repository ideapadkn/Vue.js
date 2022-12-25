<template>
  <div class="navbar">
    <div
      @click="$router.push('/')" 
      class="title"
    >
      Vue 3
    </div>
    <div class="navbar__btns">
      <my-button
        @click="$router.push('/users')"
      >
        Users
      </my-button>
      <my-button
        style="margin-left: 15px;"
        @click="$router.push('/about')"
      >
        About
      </my-button>
    </div>
  </div>
</template>

<script>
  export default {
    
  }
</script>

<style scoped>
.title {
  cursor: pointer;
  align-items: center;
  display: block;
  font-weight: bold;
  font-size: 36px;
}
.navbar {
  height: 50px;
  background-color: rgb(230, 231, 231);
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}
.navbar__btns {
  margin-left: auto;
}
</style>