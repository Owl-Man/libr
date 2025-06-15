<template>
  <transition name="scale-fade">
    <div v-if="isOpen" class="collection-popup-overlay" @click="onClose">
      <div class="collection-popup" @click.stop>
        <button class="close-button" @click="onClose">&times;</button>
        <h2 class="collection-title">{{ collection?.title }}</h2>
        <div class="books-grid">
          <div v-for="book in collection?.books || []" :key="book.title" class="book-card cursor-pointer" @click="$emit('book-click', book)">
            <img :src="book.cover" :alt="book.title" class="book-cover" />
            <div class="title">{{ book.title }}</div>
            <div class="author">{{ book.author }}</div>
            <div class="rating">{{ book.rating }} ★</div>
            <div class="chapter">{{ book.chapters || book.description }}</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'CollectionPopup',
  props: {
    isOpen: {
      type: Boolean,
      required: true
    },
    collection: {
      type: Object,
      required: false,
      default: () => ({})
    },
    onClose: {
      type: Function,
      required: true
    }
  }
}
</script>

<style scoped lang="scss">
@import "~assets/scss/_book-card.scss";
.scale-fade-enter-active,
.scale-fade-leave-active {
  transition: opacity 0.25s cubic-bezier(.4,0,.2,1), transform 0.25s cubic-bezier(.4,0,.2,1);
}
.scale-fade-enter-from,
.scale-fade-leave-to {
  opacity: 0;
  transform: scale(0.92);
}
.collection-popup-overlay {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2000;
}
.collection-popup {
  background: #fff;
  border-radius: 14px;
  padding: 32px 32px 24px 32px;
  min-width: 400px;
  max-width: 900px;
  width: 90vw;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
}
.close-button {
  position: absolute;
  top: 18px;
  right: 18px;
  background: none;
  border: none;
  font-size: 32px;
  cursor: pointer;
  color: #666;
  transition: transform 0.2s;
  padding: 0;
  line-height: 1;
}
.close-button:hover {
  color: #000;
  transform: scale(1.2);
}
.collection-title {
  margin-top: 0;
  margin-bottom: 28px;
  font-size: 2rem;
  text-align: center;
  color: #333;
}
.books-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 24px;
}
</style> 