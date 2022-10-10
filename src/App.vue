<template>
    <n-message-provider>
        <router-view />
    </n-message-provider>
</template>

<script lang="ts" setup>
import { NMessageProvider } from 'naive-ui';
</script>
>

<style lang="less">
#app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
    box-sizing: border-box;
}

#nav {
    padding: 30px;

    a {
        font-weight: bold;
        color: #2c3e50;

        &.router-link-exact-active {
            color: #42b983;
        }
    }
}
</style>
