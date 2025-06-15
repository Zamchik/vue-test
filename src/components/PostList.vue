<template>
    <div v-if="posts.length > 0">
        <h3>Список пользователей</h3>
        <PostItem v-for="post in posts" 
        :post="post" 
        :key="post.id" 
        @remove="$emit('remove', post)" />
    </div>
    <h2 v-else style="color: red;">
        Список постов пуст
    </h2>
</template>

<script>
import PostItem from './PostItem.vue';

export default {
    components: { PostItem },
    props: {
        posts: {
            type: Array,
            required: true
        }
    }
}




</script>

<style scoped>
.post {
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
}
</style>