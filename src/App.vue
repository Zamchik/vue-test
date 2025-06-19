<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <div class="app__btns">
            <MyButton @click="showDialog">
                Создать пост
            </MyButton>
            <MySelecte v-model="selectedSort" :options="sortOptions" />
        </div>
        <input type="text" v-model.trim="modificatorValue">
        <MyDialog v-model:show="dialogVisible">
            <PostForm @create="createPost" />
        </MyDialog>
        <PostList :posts="sortedPosts" @remove="removePost" v-if="!isPostLoading" />
        <div v-else>Идёт загрузка...</div>
    </div>
</template>

<script>
import axios from 'axios';
import PostForm from './components/PostForm.vue';
import PostList from './components/PostList.vue';
import MyButton from './components/UI/MyButton.vue';
import MyDialog from './components/UI/MyDialog.vue';
import MySelecte from './components/UI/MySelecte.vue';
export default {
    components: {
        PostList, PostForm
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            modificatorValue: "",
            isPostLoading: false,
            selectedSort: "",
            sortOptions: [
                { value: "title", name: "По названию" },
                { value: "body", name: "По содержимому" }
            ]
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
                this.isPostLoading = true;
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10");
                this.posts = response.data;
            } catch (error) {
                alert("Error")
            } finally {
                this.isPostLoading = false;
            }
        }
    },
    mounted() {
        this.fetchPost()
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => {
                return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
            })
        }
    },
    // watch: {
    //     selectedSort(newValue) {
    //         this.posts.sort          
    //     }
    // }
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

.app__btns {
    display: flex;
    justify-content: space-between;
    margin: 15px 0;
}
</style>