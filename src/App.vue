<template>
    <div id="app">
        <login></login>
        <keep-alive>
            <router-view></router-view>
        </keep-alive>
    </div>
</template>

<script>
import Login from 'components/login.vue'

export default {
    components: {
        Login
    }
}
</script>

