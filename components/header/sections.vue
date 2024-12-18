<template>
    <div class="sections">
        <div
            v-for="item in categories"
            :key="item.semanticId"
        >
            <nuxt-link
                :to="`/${item.semanticId}`"
            >
                <button class="sectionsBtn" type="button">{{ item.name }}</button> 
            </nuxt-link>
        </div>
    </div>
</template>
<script>

const DEFAULT_CATEGORY = '1';

export default{
    props:{
        categories:{
            type: Array,
            required: true
        }
    }
}
</script>

<style lang="scss">
.sections {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 1000px;
}
.sectionsBtn {
    font-size: $headerFontSize;
}
</style>