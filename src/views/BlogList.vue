<template>
    <div>
        <h1>Blogul GGIT FD</h1>

        <div v-if="bloglist.length > 0">
            <BlogItem v-for="blog in bloglist" :key="blog.id" :id="blog.id" :user="blog.user" :post="blog.post"
                :likes="blog.likes" :tag="blog.tag"
                @like="likeHandler($event)" 
                @dislike="dislikeHandler($event)"
                />
               
        </div>
        <div v-else>
            <p>Nici un post nu a fost creat</p>
        </div>
        <div>
            <input v-model="user" />
            <textarea v-model="post"></textarea>
            <select v-model="tag"> 
                <option value="general">General</option>
                <option value="flame">Flame</option>
                <option value="recommandation">Recommandation</option>
            </select>
            
        </div>
            <button @click="submitForumHandler">Save</button>
        </div>
</template>

<script>
import BlogItem from '../components/BlogItem.vue';
import { myaxios } from '../axios'
export default {
    components: { BlogItem },
    data() {
        return {
            user: "",
            post: "",
            tag: "",
            bloglist: []
        }
    },
    methods: {
        submitForumHandler(event) {
            const newBlog = {
                user: this.user,
                post: this.post,
                tag: this.tag
            } 

            //Trimitem la BackEnd

            myaxios.post("/blog", newBlog).then((response) => {
                this.bloglist = response.data.blogs
            })
            console.log(newBlog)
        },
        fetchBlogPosts(){
            myaxios.get("/blog").then(
                (response) => {
                    console.log(response.data.blogs)
                    this.bloglist = response.data.blogs
                }
            )
        },
        likeHandler(event){
            console.log(event.itemId)
            myaxios.post(`/blog/${event.itemId}/like`).then((response) => {
                console.log(response.data.blogs)
                this.bloglist = response.data.blogs
            })
        },
        dislikeHandler(event) {
            console.log(event.itemId)
            myaxios.post(`/blog/${event.itemId}/dislike`).then((response) => {
                console.log(response.data.blogs)
                this.bloglist = response.data.blogs
        })
    },
},
    mounted(){
        this.fetchBlogPosts();
    },
}

</script>

<style lang="scss" scoped></style>
