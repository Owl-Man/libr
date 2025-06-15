<template>
  <transition name="fade">
    <div v-if="isOpen" class="add-to-collection-popup-overlay" @click="closePopup">
      <div class="add-to-collection-popup" @click.stop>
        <button class="close-button" @click="closePopup">&times;</button>
        <h2>Добавить в коллекцию</h2>
        <div v-if="collections.length === 0" class="no-collections-message">
          <p>У вас пока нет коллекций. Создайте новую коллекцию на вкладке "Коллекции".</p>
        </div>
        <div v-else class="collection-list">
          <div 
            v-for="collection in collections" 
            :key="collection.id"
            class="collection-item"
            @click="selectCollection(collection)"
          >
            <img :src="collection.image" :alt="collection.title" class="collection-item-image" />
            <p>{{ collection.title }}</p>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'AddToCollectionPopup',
  props: {
    isOpen: {
      type: Boolean,
      required: true
    },
    bookToAdd: {
      type: Object,
      required: true
    },
    collections: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    closePopup() {
      this.$emit('close');
    },
    selectCollection(collection) {
      this.$emit('add-to-collection', { book: this.bookToAdd, collectionId: collection.id });
      this.closePopup();
    }
  }
}
</script>

<style lang="scss" scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.add-to-collection-popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 4000; /* Higher than book popup */
  backdrop-filter: blur(5px);
}

.add-to-collection-popup {
  background-color: white;
  border-radius: 12px;
  padding: 25px;
  max-width: 500px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
  position: relative;
  animation: popup-appear 0.3s ease-out;

  h2 {
    margin-top: 0;
    margin-bottom: 20px;
    font-size: 24px;
    color: #333;
    text-align: center;
  }
}

.close-button {
  position: absolute;
  top: 15px;
  right: 15px;
  background: none;
  border: none;
  font-size: 30px;
  cursor: pointer;
  color: #666;
  transition: transform 0.2s ease;
  &:hover {
    color: #000;
    transform: scale(1.2);
  }
}

.no-collections-message {
  text-align: center;
  color: #666;
  padding: 20px;
  border: 1px dashed #ccc;
  border-radius: 8px;
}

.collection-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.collection-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 10px 15px;
  border: 1px solid #eee;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s ease, box-shadow 0.2s ease;

  &:hover {
    background-color: #f9f9f9;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  }

  .collection-item-image {
    width: 50px;
    height: 50px;
    border-radius: 6px;
    object-fit: cover;
    flex-shrink: 0;
  }

  p {
    margin: 0;
    font-size: 16px;
    color: #333;
    font-weight: 500;
  }
}
</style> 