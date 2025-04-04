<template>
  <div>
    <CatalogPage :initial-query="$route.params.query || ''" />
  </div>
</template>

<script>
import CatalogPage from '@/components/Catalog.vue';

export default {
  name: 'CatalogPageWithQuery',
  components: {
    CatalogPage,
  },
};
</script>