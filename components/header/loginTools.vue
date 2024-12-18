<template>
    <div class="login-tools">
        <Profile v-if="isUserFetched"/>
        <Login v-else/>
    </div>
</template>
<script>
import Login from '@/components/header/login';
import Profile from '@/components/header/profile';

export default {
    name: "Default",

    components: {
        Login,
        Profile
    },
    props: {
        isDesktop: Boolean
    },
    data() {
        return {
            isUserFetched: true
        }
    }
}
</script>

<style lang="scss">
.login-tools {
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>