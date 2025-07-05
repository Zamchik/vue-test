<template>
    <div>
        <h1>Страница с постами</h1>
        <MyInput v-focus :model-value="searchQuery" @update:model-value="setSearchQuery" placeholder="Поиск..." />
        <div class="app__btns">
            <MyButton @click="showDialog">
                Создать пост
            </MyButton>
            <MySelecte model-value="selectedSort" @update:model-value="setSelectedSort" :options="sortOptions" />
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
import { mapState, mapActions, mapGetters, mapMutations } from 'vuex';
export default {
    components: {
        PostList, PostForm, PostList, MyButton, MyDialog, MySelecte, MyInput,
    },
    data() {
        return {
            dialogVisible: false,
        }
    },
    methods: {
        ...mapMutations({
            setPage: 'post/setPage',
            setSearchQuery: 'post/setSearchQuery',
            setSelectedSort: 'post/setSelectedSort'
        }),
        ...mapActions({
            loadMorePosts: 'post/loadMorePosts',
            fetchPosts: 'post/fetchPosts'
        }),
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
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        ...mapState({
            posts: state => state.post.posts,
        modificatorValue: state => state.post.modificatorValue,
        isPostLoading: state => state.post.isPostLoading,
        selectedSort: state => state.post.selectedSort,
        searchQuery: state => state.post.searchQuery,
        page: state => state.post.page,
        limit: state => state.post.limit,
        totalPages: state => state.post.totalPages,
        sortOptions: state => state.post.sortOptions
        }),
        ...mapGetters({
            sortedPost: 'post/sortedPost',
            sortedAndSearchedPosts: 'post/sortedAndSearchedPosts',
        })
        // sortedPosts() {
        //     return [...this.posts].sort((post1, post2) => {
        //         return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
        //     })
        // },
        // sortedAndSearchedPosts() {
        //     return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        // }
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