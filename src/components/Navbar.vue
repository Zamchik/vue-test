<template>
    <div class="navbar">
        <div @click="$router.push('/')">Vue 3</div>
        <div class="navbar__btns">
            <MyButton @click="$router.push('/post')" style="margin-right: 28px;">Посты</MyButton>
            <MyButton @click="$router.push('/about')">О сайте</MyButton>
            <MyButton @click="$router.push('/store')">Store</MyButton>
            <MyButton @click="$router.push('/composition')">CompositionAPI</MyButton>
        </div>
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style scoped>
.navbar {
    height: 50px;
    background-color: lightgrey;
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
    padding: 0 15px;
}

.navbar__btns {
    margin-left: auto;
}
</style>