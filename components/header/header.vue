<template>
    <div class="header">
        <Logo />
        <Sections
            :categories="headerSections"
            @change-layout="onChangeLayout"
        />
        <LoginTools 
            v-if="isUserFetched"
            :isDesktop="isDesktop"
        />
    </div>
</template>

<script>
import Logo from '@/components/header/logo';
import Sections from './sections.vue';

export default {
    name: "Default",

    components: {
        Logo,
        Sections,
        LoginTools: () => import(`@/components/header/loginTools`)
    },

    data() {
        return {
            isDesktop: true,
            isUserFetched: true,
            headerSections: [
                {"name": "Обзор", "semanticId": "1"},
                {"name": "Каталог", "semanticId": "2"},
                {"name": "Коллекции", "semanticId": "3"},
                {"name": "Закладки", "semanticId": "4"}
            ]
        }
    },
    methods: {
        onChangeLayout(layout) {
            this.$emit('change-layout', layout);
        }
    }
}
</script>

<style lang="scss">
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.7rem;
    background-color: $mainColor;
}
</style>