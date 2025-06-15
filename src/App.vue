<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <input type="text" v-model.trim="modificatorValue">
        <MyButton @click="showDialog" style="margin: 15px 0;">
            Создать пост
        </MyButton>
        <MyDialog v-model:show="dialogVisible">
            <PostForm @create="createPost" />
        </MyDialog>
        <PostList :posts="posts" @remove="removePost" />
    </div>
</template>

<script>
import axios from 'axios';
import PostForm from './components/PostForm.vue';
import PostList from './components/PostList.vue';
import MyButton from './components/UI/MyButton.vue';
import MyDialog from './components/UI/MyDialog.vue';
export default {
    components: {
        PostList, PostForm
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            modificatorValue: "",
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },
        inputTitle(event) {
            this.title = event.target.value;
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisible = true
        },
        async fetchPost() {
            try {
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10");
                this.posts = response.data;
                console.log(response)
            } catch (error) {
                alert("Error")
            }
        }
    }
}
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.app {
    padding: 20px;
}

.form {
    display: flex;
    flex-direction: column;
}
</style>