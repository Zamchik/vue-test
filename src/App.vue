<template>
    <Navbar></Navbar>
    <div class="app">
        <RouterView></RouterView>
    </div>
</template>

<script>
import { RouterView } from 'vue-router';
import Navbar from './components/Navbar.vue';

    export default {
        components: {
            Navbar,
        }
    }
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.app {
    padding: 20px;
}
</style>