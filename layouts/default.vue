<!-- layouts/default.vue -->
<template>
  <div class="page-in">
    <Header @change-layout="changeLayout" />
    <div class="content">
      <Layout1 v-if="currentLayout === '1'" />
      <Layout2 v-if="currentLayout === '2'" />
      <Layout3 v-if="currentLayout === '3'" />
      <Layout4 v-if="currentLayout === '4'" />
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

export default {
  name: 'Default',
  components: {
    Header,
    Footer,
    Layout1,
    Layout2,
    Layout3,
    Layout4
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