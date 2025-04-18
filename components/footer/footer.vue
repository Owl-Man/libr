<template>
    <footer class="footer">
        <div class="footer__copyright">
            {{ copyrightText }}
        </div>
    </footer>
</template>


<script>
export default{
    computed: {
        copyrightText() {
            const currentYear = new Date().getFullYear();
            return `@ 2023-${currentYear}, Libria`;
        }
    }
}
</script>


<style lang="scss">
.footer {
    background: $mainColor;
}
</style>