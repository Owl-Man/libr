<template>
    <nuxt-link class="logo" :to="``">
        <picture class="logo">
            <source srcset="booksIconMobile.png" media="(max-width: 330px)" width=30px height=30px>
            <source srcset="booksIconDesktop.png" media="(min-width: 340px)" width=50px height=50px>
            <img src="booksIconDesktop.png">
        </picture>
    </nuxt-link>
</template>

<style lang="scss">
.logo {
    position: relative;
    display: flex;
    justify-content: space-between;
    z-index: $zMain;
    transition: transform 0.3s ease;
}

.logo:hover {
    transform: scale(1.07);
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
}
</style>