<template>
    <div>
        <h1>Страница с постами</h1>
        <MyInput v-focus v-model="searchQuery" placeholder="Поиск..." />
        <div class="app__btns">
            <MyButton @click="showDialog">
                Создать пост
            </MyButton>
            <MySelecte v-model="selectedSort" :options="sortOptions" />
        </div>
        <!-- <input type="text" v-model.trim="modificatorValue"> -->
        <MyDialog v-model:show="dialogVisible">
            <PostForm @create="createPost" />
        </MyDialog>
        <PostList :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostLoading" />
        <div v-else>Идёт загрузка...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
        <!-- <div class="page__wrapper">
            <div v-for="pageNumber in totalPages" :key="pageNumber" class="page" :class="{
                'current_page': page === pageNumber
            }" @click="changePage(pageNumber)">
                {{ pageNumber }}
            </div>
        </div> -->
    </div>
</template>

<script>
import axios from 'axios';
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import MyButton from '@/components/UI/MyButton.vue';
import MyDialog from '@/components/UI/MyDialog.vue';
import MySelecte from '@/components/UI/MySelecte.vue';
import MyInput from '@/components/UI/MyInput.vue';
export default {
    components: {
        PostList, PostForm, PostList, MyButton, MyDialog, MySelecte, MyInput,
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            modificatorValue: "",
            isPostLoading: false,
            selectedSort: "",
            searchQuery: "",
            page: 1,
            limit: 18,
            totalPages: 0,
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
        // changePage(pageNumber) {
        //     this.page = pageNumber
        // },
        async fetchPost() {
            try {
                this.isPostLoading = true;
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = response.data;
            } catch (error) {
                alert("Error")
            } finally {
                this.isPostLoading = false;
            }
        },
        async loadMorePosts() {
            try {
                this.page += 1;
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = [...this.posts, ...response.data];
            } catch (error) {
                alert("Error")
            }
        }
    },
    mounted() {
        this.fetchPost()
        // console.log(this.$refs.observer)
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => {
                post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
            })
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    watch: {
        // page() {
        //     this.fetchPost()
        // }
    }
}
</script>

<style scoped>
.form {
    display: flex;
    flex-direction: column;
}

.app__btns {
    display: flex;
    justify-content: space-between;
    margin: 15px 0;
}

.page__wrapper {
    display: flex;
    margin-top: 15px;
}

.page {
    border: 1px solid black;
    padding: 10px;
}

.current_page {
    border: 2px solid teal;
}

.observer {
    height: 30px;
    background: gray;
}
</style>