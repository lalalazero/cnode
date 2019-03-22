<template>
    <div class="PostList">
        <div class="loading" v-if="isLoading">
            <img src="../assets/loading.gif" alt="">
        </div>
        <div class="posts" v-if="!isLoading">
            <ul>
                <li>
                    <div class="topbar">
                        <span class="active">全部</span>
                        <span>精华</span>
                        <span>分享</span>
                        <span>问答</span>
                        <span>招聘</span>
                    </div>
                </li>
                <li v-for="post in posts">
                    <img :src="post.author.avatar_url" alt="">
                    <span>
                        <span class="reply_count">{{post.reply_count}}</span><span class="visit_count">/{{post.visit_count}}</span>
                    </span>
                    <span :class="[{put_good:(post.good === true), put_top:(post.top === true), 
                        'topiclist-tab':(post.good !== true && post.top !== true)}]">
                        <span>{{ post | formatTab }}</span>
                    </span>
                    <span class="title">
                        {{post.title}}
                    </span>
                    <span class="last_reply-at">
                        {{post.last_reply_at | formatDate}}
                    </span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'PostList',
        data(){
            return {
                isLoading: true,
                posts: []
            }
        },
        beforeMount(){
            this.isLoading = true
            this.getData()
        },
        methods: {
            getData(){
                this.$http.get('https://cnodejs.org/api/v1/topics',{
                    page: 1,
                    limit: 20
                }).then((res)=>{
                    this.isLoading = false
                    console.log(res)
                    if (res.data.success) {
                        this.posts = res.data.data
                    }
                }).catch(err => {
                    console.log('请求失败,err ', err)
                })
            }
        }
    }
</script>

<style scoped>
    img {
        width: 30px;
        height: 30px;
        border-radius: 2px;
        margin-right: 5px;
    }
    ul {
        list-style: none;
    }
    .topbar {
        color: #5eb700;
        background-color: #f6f6f6;
        padding: 10px 20px;
        font-size: 15px;
    }
    .topbar span {
        padding: 2px 5px;
        border-radius: 2px;
        margin-right: 10px;
    }
    .topbar span.active {
        color: white;
        background-color:#5eb700; 
    }
    li:not(:first-child) {
        padding: 10px;
        background: white;
        display: flex;
        align-items: center;
        border-bottom: 1px solid #f0f0f0;
    }
    li:not(:first-child):hover {
        background-color: #f6f6f6;
    }
    .put_good, .put_top {
        color: white;
        background-color: #5eb700;
        font-size: 12px;
        padding: 2px 4px;
        border-radius: 3px;
        margin: 0 4px;
    }
    .topiclist-tab {
        color: #999;
        background-color: #e5e5e5;
        font-size: 12px;
        padding: 2px 4px;
        border-radius: 3px;
        margin: 0 4px;
    }
    .reply_count {
        font-size: 14px;
        color: #9e78c0;
    }
    .visit_count {
        font-size: 12px;
        color: #b4b4b4;
    }
    .title {
        white-space: nowrap;
        max-width: 70%;
        text-overflow: ellipsis;
        overflow: hidden;
    }
    .title:hover{
        cursor: pointer;
        text-decoration: underline;
    }
    .last_reply-at {
        font-size: 12px;
        color: #777;
        margin-left: auto;
    }
</style>