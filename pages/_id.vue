<template>
    <div>
        {{ post.title }}
        {{ $common.beautifulTime(post.timestamp) }}
        {{ post.content }}
        <el-input type="text" v-model="edittitle" />
        <!-- <div>
            <input type="text" v-model="date" />
        </div> -->
        <el-input type="text" v-model="editcontent" />
        <el-button @click="editpost">修改</el-button>
        <div v-for="comment in comments" :key="comment.id">
            {{ comment.content }}
            {{ $common.beautifulTime(comment.timestamp) }}
            <el-button @click="deletecomment(comment.id)">删评</el-button>
        </div>
        <el-input type="text" v-model="comment" />
        <el-button @click="editcomment">发表评论</el-button>
    </div>
</template>


<script>
import Vue from 'vue'
import axios from 'axios'
export default Vue.extend({
    data() {
        return {
            edittitle: '',
            editcontent: '',
            comment: '',
        }
    },
    async asyncData({ route }) {
        // console.log(route)
        let post = {}
        let res = axios.get(`http://127.0.0.1:5000/api/post/` + route.params.id)
        let comments = axios.get(
            `http://127.0.0.1:5000/api/post/comment/` + route.params.id
        )
        res = await res
        comments = await comments
        if (res.status == 200) {
            // console.log(res.data)
            post = res.data
        }
        if (comments.status == 200) {
            // console.log(comments.data)
            comments = comments.data.results
        }
        return {
            post,
            comments,
        }
    },
    methods: {
        async editpost() {
            // console.log(this.$route)
            let res = await axios.put(
                `http://127.0.0.1:5000/api/post/` + this.$route.params.id,
                {
                    title: this.edittitle,
                    content: this.editcontent,
                }
            )
            if (res.status == 200) {
                // console.log(res.data)
                // this.post = res.data
                this.edittitle = ''
                this.editcontent = ''
                this.postreget()
                alert('修改成功')
            } else {
                alert('修改失败')
            }
        },
        async postreget() {
            let res = axios.get(
                `http://127.0.0.1:5000/api/post/` + this.$route.params.id
            )
            let newcomments = axios.get(
                `http://127.0.0.1:5000/api/post/comment/` +
                    this.$route.params.id
            )
            res = await res
            newcomments = await newcomments
            if (res.status == 200) {
                // console.log(res.data)
                this.post = res.data
            }
            if (newcomments.status == 200) {
                // console.log(res.data)
                this.comments = newcomments.data.results
            }
        },
        async editcomment() {
            let res = await axios.post(
                `http://127.0.0.1:5000/api/post/comment/` +
                    this.$route.params.id,
                {
                    content: this.comment,
                }
            )
            if (res.status == 200) {
                this.postreget()
                alert('评论成功')
            }
        },
        async deletecomment(id) {
            // console.log(id)
            let res = await axios.delete(
                `http://127.0.0.1:5000/api/post/comment/` + id
            )
            if (res.status == 200) {
                this.postreget()
                alert('删评成功')
            }
        },
    },
    mounted() {
        // console.log(this.$route)
    },
})
</script>