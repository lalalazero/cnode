<template>
  <div class="autherinfo">
    <div class="author_summary">
      <div class="topbar">作者</div>
      <router-link :to="{
          name: 'user_info',
          params: {
              name: userinfo.loginname
          }
      }">
          <img :src="userinfo.avatar_url" alt="">
          <span> {{ userinfo.loginname }}</span>
          
      </router-link>
      <p>积分: {{ userinfo.score}}</p>
      
    </div>
    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <ul>
          <li v-for="item in topicLimitBy5">
          <router-link
            :to="{
                      name: 'post-content',
                      params: {
                          id: item.id,
                          name: item.author.loginname
                      }
                  }"
          >{{item.title}}</router-link>
        </li>
      </ul>
    </div>
    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <ul>
          <li v-for="item in userinfo.recent_replies">
            <router-link
              :to="{
                      name: 'post-content',
                      params: {
                          id: item.id,
                          name: item.author.loginname
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
  name: "SlideBar",
  data() {
    return {
      userinfo: {
          recent_topics: [],
          recent_replies: []
      }
    };
  },
  beforeMount(){
      this.getData()
  },
  computed: {
      topicLimitBy5() {
          if(this.userinfo.recent_topics.length > 5) {
              return this.userinfo.recent_topics.slice(0, 5)
          }else{
              return this.userinfo.recent_topics;
          }
      },
      repliesLimitBy5() {
          if(this.userinfo.recent_replies.length > 5) {
              return this.userinfo.recent_replies.slice(0, 5)
          }else{
              return this.userinfo.recent_replies;
          }
      }
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
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
.autherinfo {
  /* border: 1px solid red; */
  float: right;
  width: 320px;
}

.autherinfo > div {
    margin-bottom: 10px;
}

.author_summary img {
    margin: 10px;
    width: 80px;
    height: 80px;
}

.author_summary p {
    padding: 10px;
}

.topbar {
  background-color: #ccc;
  padding: 10px 5px;
}
.author_summary, .recent_topics, .recent_replies {
    background-color: #fff;
}
.recent_replies ul, .recent_topics ul{
    padding: 10px;
}

.recent_replies ul>li, .recent_topics ul>li {
    padding: 5px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    /* border-bottom: 1px solid #ccc; */
}
</style>



