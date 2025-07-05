<template>
    <div>
        <h1>Страница с постами</h1>
        <MyInput v-focus v-model="searchQuery" placeholder="Поиск..." />
        <div class="app__btns">
            <MyButton>
                Создать пост
            </MyButton>
            <MySelecte v-model="selectedSort" :options="sortOptions" />
        </div>
        <MyDialog v-model:show="dialogVisible">
            <PostForm />
        </MyDialog>
        <PostList :posts="sortedAndSearchedPosts" v-if="!isPostLoading" />
        <div v-else>Идёт загрузка...</div>
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
import { ref } from 'vue';
import { usePosts } from '@/hooks/usePosts';
import useSortedPosts from '@/hooks/useSortedPosts';
import useSortedAndSearchedPosts from '@/hooks/useSortedAndSearchedPosts';
export default {
    components: {
        PostList, PostForm, MyButton, MyDialog, MySelecte, MyInput,
    },
    data() {
        return {
            dialogVisible: false,
            sortOptions: [
                { value: "title", name: "По названию" },
                { value: "body", name: "По содержимому" }
            ]
        }
    },
    setup(props) {
        const { posts, isPostLoading, totalPages } = usePosts(10);
        const { sortedPosts, selectedSort } = useSortedPosts(posts);
        const { searchQuery, sortedAndSearchedPosts } = useSortedAndSearchedPosts(sortedPosts)
        return {
            posts,
            totalPages,
            isPostLoading,
            searchQuery,
            sortedAndSearchedPosts,
            selectedSort
        }
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