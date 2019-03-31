<template>
  <div id="app">
    <Header />
    <main>
      <router-view name='slidebar'></router-view>
      <div class="main-content">
        <router-view name='main'></router-view>
      </div>
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
    width: 90%;
    margin: auto;
    /* border: 1px solid blue; */
  }
  .main-content {
    margin-right: 330px;
    /* border: 1px solid red; */
  }
</style>
