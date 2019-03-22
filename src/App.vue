<template>
  <div id="app">
    <Header />
    <main>
      <PostList />
    </main>
    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import PostList from './components/PostList.vue'
export default {
  name: 'App',
  components: {
    Header,
    PostList
  }
}
</script>

<style scoped>
  #app {
    background-color: #ddd;
  }
  main {
    width: 80%;
    margin: auto;
  }
</style>
