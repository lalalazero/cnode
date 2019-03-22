<template>
    <div>
        <div class="loading" v-if="isLoading">
            <img src="../assets/loading.gif" alt="">
        </div>
        <div class='article' v-else>
            <div>
                <div class="topic_title">
                    {{ post.title }}
                </div>
                <ul>
                    <li>•发布于: {{ post.create_at | formatDate }}</li>
                    <li>• 作者:
                        {{ post.author.loginname}}
                    </li>
                    <li>• {{ post.visit_count }}次浏览</li>
                    <li>• 来自{{ post | formatTab}}</li>
                </ul>
                <div class="topic-content" v-html="post.content"></div>
            </div>
            <div class="reply">
                <div>回复</div>
                <div v-for="(reply, index) in post.replies">
                    <img :src="reply.author.avatar_url" alt="">
                    <span>{{ reply.author.loginname }}</span>
                    <span>{{ index + 1}}楼</span>
                    <span v-if="reply.ups.length > 0">{{ reply.ups.length }}</span>
                    <span v-else></span>
                    <p v-html="reply.content"></p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Article',
        data(){
            return {
                isLoading: true,
                post: {},
            }
        },
        methods: {
            getArticleData(){
                this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`).then(res => {
                    this.isLoading = false
                    if (res.data.success) {
                        this.post = res.data.data
                    }
                }).catch(err => {
                    console.log('err ', err)
                })
            }
        },
        beforeMount(){
            this.getArticleData()
        }
    }
</script>

<style scoped>

    ul {
        list-style: none;
    }
</style>