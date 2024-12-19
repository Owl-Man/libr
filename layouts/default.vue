<template>
    <div class="page-main">
        <Header />
        <nuxt :key="$route.fullPath" class="content"/>
        <Footer class="footer"/>
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
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    position: relative;
    z-index: $zMain;
    font-family: $main-font-family;
}

.content {
  flex: 1;
}

.footer {
    padding: 10px;
}
</style>