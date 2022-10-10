<template>
    <div class="home">
        <h1>GENSHIN UID PLUGIN FRONTEND</h1>
        <router-link to="/user-info"> USER INFO PAGE </router-link>
    </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
// @ is an alias to /src

@Options({
    components: {},
})
export default class HomeView extends Vue {}
</script>
