<template>
    <div class="page-main">
        <Header />
        <nuxt :key="$route.fullPath" />
        <Footer />
    </div>
</template>

<script>
import Header from '@/components/header/header';
import Footer from '@/components/footer/footer';

export default {
    name: "Default",

    components: {
        Header,
        Footer
    }
}
</script>


<style lang="scss">
.page-main {
    position: relative;
    z-index: $zMain;
    font-family: $main-font-family;
}
</style>