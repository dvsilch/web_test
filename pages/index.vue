<template>
    <div class="container">
        <!-- <input type="text" v-model="before" />
        <button @click="plus">提交</button>
        <div>before:{{ before }}</div>
        <div>after:{{ after }}</div> -->
        <!-- <button @click="getPost">获取帖子</button> -->
        <div v-for="post in posts" :key="post.id">
            <nuxt-link :to="{ name: 'id', params: { id: post.id } }">
                {{ post.title }}
                {{ $common.beautifulTime(post.timestamp) }}
                {{ post.content }}
            </nuxt-link>
            <el-button @click="deletepost(post.id)">删除</el-button>
        </div>
        <div>
            <el-input type="text" v-model="title" />
        </div>
        <div>
            <el-input type="text" v-model="content" />
        </div>
        <el-button @click="postpost">发布</el-button>
        <!-- <div style="display: block">
            <nuxt-link to="/css-animation/Staggered_Wave_Loading">
                Staggered_Wave_Loading
            </nuxt-link>
            <nuxt-link to="/css-animation/Staggered_LandIn_Text">
                Staggered_LandIn_Text
            </nuxt-link>
        </div> -->
    </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
export default Vue.extend({
    data() {
        return {
            // before: null,
            // after: '',
            title: '',
            // date: '',
            content: '',
        }
    },
    async asyncData() {
        let posts = []
        let res = await axios.get(`http://127.0.0.1:5000/api/post`)
        if (res.status == 200) {
            // console.log(res.data)
            posts = res.data.results
        }
        return {
            posts,
        }
    },
    methods: {
        // async plus() {
        //     let response = await axios.post(`http://127.0.0.1:5000/api`, {
        //         before: this.before,
        //     })
        //     if (response.status == 200) {
        //         console.log(response.data)
        //         this.after = response.data.after
        //     }
        // },
        async deletepost(idx) {
            // console.log(idx)
            let res = await axios.delete(
                `http://127.0.0.1:5000/api/post/${idx}`
            )
            if (res.status == 200) {
                this.postget()

                alert('删除成功')
            } else {
                alert('删除失败')
            }
        },
        async postpost() {
            let res = await axios.post(`http://127.0.0.1:5000/api/post`, {
                title: this.title,
                // date: this.date,
                content: this.content,
            })
            if (res.status == 200) {
                this.postget()
                alert('发布成功')
            } else {
                alert('发布失败')
            }
        },
        async postget() {
            let res = await axios.get(`http://127.0.0.1:5000/api/post`)
            if (res.status == 200) {
                // console.log(res.data)
                this.posts = res.data.results
                console.log(this.posts)
            }
        },
    },
    mounted() {
        // console.log(this.$route.name)
    },
})
</script>
