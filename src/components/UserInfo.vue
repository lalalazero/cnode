
<template>
  <div class="userInfo">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt>
    </div>
    <div class="userInfomation">
      <section>
        <img :src="userinfo.avatar_url" alt>
        <span>{{userinfo.loginname}}</span>
        <p>{{userinfo.score}}积分</p>
        <p>注册时间{{userinfo.create_at | formatDate }}</p>
      </section>
      <div class="replies">
        <p>回复的主题</p>
        <ul>
          <li v-for="item in userinfo.recent_replies">
            <router-link
              :to="{
                      name: 'post-content',
                      params: {
                          id: item.id
                      }
                  }"
            >{{item.title}}</router-link>
          </li>
        </ul>
      </div>
    </div>
    <div class="topics">
      <p>创建的主题</p>
      <ul>
        <li v-for="item in userinfo.recent_topics">
          <router-link
            :to="{
                      name: 'post-content',
                      params: {
                          id: item.id
                      }
                  }"
          >{{item.title}}</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserInfo",
  data() {
    return {
      isLoading: false,
      userinfo: {}
    };
  },
  beforeMount() {
    this.isLoading = true;
    this.getData();
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false;
          console.log(res);
          if (res.data.success) {
            this.userinfo = res.data.data;
          }
        })
        .catch(err => {
          console.log("请求失败,err ", err);
        });
    }
  }
}
</script>

<style scoped>
</style>

