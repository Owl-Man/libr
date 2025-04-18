<template>
  <div class="page-main">
    <Header />
    <transition name="fade" mode="out-in">
      <nuxt :key="$route.fullPath" class="content" />
    </transition>
    <Footer class="footer" />
  </div>
</template>

<script>
import Header from '@/components/header/header';
import Footer from '@/components/footer/footer';

export default {
  name: 'Default',
  components: {
    Header,
    Footer,
  },
  watch: {
    // Отслеживаем изменения маршрута для предотвращения лишних перерендеров
    $route(to, from) {
      // Можно добавить логику для управления состоянием при смене маршрута
      this.handleRouteChange(to, from);
    },
  },
  methods: {
    handleRouteChange(to, from) {
      // Здесь можно добавить дополнительную логику, например, прокрутку вверх
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },
  },
};
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

// Анимация для плавного перехода между страницами
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.footer {
  padding: 10px;
}
</style>