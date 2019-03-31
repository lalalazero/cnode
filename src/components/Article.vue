<template>
  <div>
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt>
    </div>
    <div class="article" v-else>
      <div class="topic">
        <div class="topbar">
          <span
            :class="[{put_good: (post.good === true), put_top: (post.top === true)}]"
          >{{ post | formatTab }}</span>
          <span class="topic_title">{{ post.title }}</span>
          <ul>
            <li>• 发布于{{ post.create_at | formatDate }}</li>
            <li>
              • 作者:
              {{ post.author.loginname}}
            </li>
            <li>• {{ post.visit_count }}次浏览</li>
            <li>• 来自 {{ post | formatTab}}</li>
          </ul>
        </div>
        <div class="topic-content" v-html="post.content"></div>
      </div>
      <div class="reply">
        <div class="topbar">回复</div>
        <ul>
          <li v-for="(reply, index) in post.replies">
            <div >
              <router-link :to="{
                name: 'user_info',
                params: {
                  name: reply.author.loginname
                }
              }">
                <img :src="reply.author.avatar_url" alt>
                <span>{{ reply.author.loginname }}</span>
              </router-link>
              <span class="floor">{{ index + 1}}楼• {{ reply.create_at | formatDate }}</span>
              <span v-if="reply.ups.length > 0">{{ reply.ups.length }}</span>
              <span v-else></span>
              <p v-html="reply.content"></p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data() {
    return {
      isLoading: true,
      post: {}
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then(res => {
          this.isLoading = false;
          if (res.data.success) {
            this.post = res.data.data;
          }
        })
        .catch(err => {
          console.log("err ", err);
        });
    }
  },
  beforeMount() {
    this.getArticleData();
  },
  // 检测路由变化
  watch: {
    '$route'(to, from){
      this.getArticleData()
    }
  }
};
</script>

<style>
@import url("../assets/markdown-github.css");

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
.article {
  /* border: 1px solid #fff; */
  margin-top: 20px;
  /* border-radius: 3px; */
}
.topic {
  background-color: #fff;
  border: 1px solid #fff;
  border-radius: 3px;
}
.topbar {
  padding: 10px;
}
.topic_title {
  display: inline-block;
  font-size: 20px;
  font-weight: bold;
  padding: 10px 0;
}
.topic li {
  display: inline-block;
  font-size: 12px;
  color: #838383;
  margin-right: 3px;
}
.topic-content {
  border-top: 1px solid #ccc;
  padding: 10px;
}

.put_good,
.put_top {
  color: white;
  background-color: #5eb700;
  font-size: 12px;
  padding: 2px 4px;
  border-radius: 3px;
  margin: 0 4px;
}

a {
  color: #08c;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
  cursor: pointer;
}

.reply .topbar {
  background-color: #f6f6f6;
  padding: 10px;
}

.reply {
  background-color: #fff;
  margin-top: 20px;
  border: 1px solid #fff;
  border-radius: 3px;
}

.reply img {
    width: 30px;
    height: 30px;
    border-radius: 2px;
}

.reply li {
    border-bottom: 1px solid #ccc; 
    padding: 10px;
}

.reply .floor {
    font-size: 14px;
    color: #08c;
}

.reply ul>li:nth-child(1),
.reply ul>li:nth-child(2),
.reply ul>li:nth-child(3),
.reply ul>li:nth-child(4) {
  background-color: #f4fcf0;
}
</style>