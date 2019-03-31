
<template>
  <div class="userInfo">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt>
    </div>
    <div class="userInfomation">
        <p class="topbar">个人信息</p>
        <img :src="userinfo.avatar_url" alt>
        <span>{{userinfo.loginname}}</span>
        <p>{{userinfo.score}}积分</p>
        <p>注册时间{{userinfo.create_at | formatDate }}</p>
    </div>
    <div class="replies">
      <p class="topbar">最近回复的主题</p>
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

    <div class="topics">
      <p class="topbar">最近创建的主题</p>
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
};
</script>

<style scoped>
ul {
    list-style: none;
    margin: 0;
    padding: 0;
}
li {
    padding: 5px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}
.topbar {
  padding: 10px;
  background: #ccc;
}
.userInfo > div {
  background-color: #fff;
  margin-bottom: 10px;
}
.userInfomation {
    padding-bottom: 5px;
    padding-left: 10px;
}
.userInfomation .topbar {
    margin-left: -10px; 
}
</style>

