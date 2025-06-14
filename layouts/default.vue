<!-- layouts/default.vue -->
<template>
  <div class="page-in">
    <Header @change-layout="changeLayout" />
    <div class="content">
      <Layout1 v-if="currentLayout === '1'" @change-layout="changeLayout" />
      <Layout2 v-if="currentLayout === '2'" @change-layout="changeLayout" />
      <Layout3 v-if="currentLayout === '3'" @change-layout="changeLayout" />
      <Layout4 v-if="currentLayout === '4'" @change-layout="changeLayout" />
      <Layout5 v-if="currentLayout === '5'" @change-layout="changeLayout" />
      <Layout6 v-if="currentLayout === '6'" @change-layout="changeLayout" />
      <Layout7 v-if="currentLayout === '7'" @change-layout="changeLayout" />
    </div>
    <Footer class="footer" />
  </div>
</template>

<script>
import Header from '@/components/header/header'
import Footer from '@/components/footer/footer'
import Layout1 from '@/layouts/1.vue'
import Layout2 from '@/layouts/2.vue'
import Layout3 from '@/layouts/3.vue'
import Layout4 from '@/layouts/4.vue'
import Layout5 from '@/layouts/5.vue'
import Layout6 from '@/layouts/6.vue'
import Layout7 from '@/layouts/7.vue'

export default {
  name: 'Default',
  components: {
    Header,
    Footer,
    Layout1,
    Layout2,
    Layout3,
    Layout4,
    Layout5,
    Layout6,
    Layout7
  },
  data() {
    return {
      currentLayout: '1' // Начальный layout
    }
  },
  methods: {
    changeLayout(layout) {
      console.log('Changing layout to:', layout)
      this.currentLayout = layout
    }
  }
}
</script>

<style scoped>
.page-in {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.content {
  flex: 1;
}
</style>