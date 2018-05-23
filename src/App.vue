<template>
    <div id="app">
        <login></login>
        <router-view></router-view>
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

