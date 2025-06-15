<template>
  <transition name="popup">
    <div v-if="isOpen" class="create-collection-popup-overlay" @click.self="$emit('close')">
      <div class="create-collection-popup-content">
        <h2 class="popup-title">Создать новую коллекцию</h2>
        <input
          v-model="collectionName"
          type="text"
          placeholder="Название коллекции"
          class="collection-name-input"
          @keyup.enter="createCollection"
        />
        <div class="popup-actions">
          <button @click="createCollection" class="create-button">Создать</button>
          <button @click="$emit('close')" class="cancel-button">Отмена</button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    isOpen: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      collectionName: ''
    };
  },
  watch: {
    isOpen(newVal) {
      if (!newVal) {
        this.collectionName = ''; // Clear input when closed
      }
    }
  },
  methods: {
    createCollection() {
      if (this.collectionName.trim()) {
        this.$emit('create', this.collectionName.trim());
        this.collectionName = '';
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.create-collection-popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.create-collection-popup-content {
  background: #fff;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  max-width: 400px;
  width: 90%;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Vue Transition Styles */
.popup-enter-active, .popup-leave-active {
  transition: opacity 0.3s ease;
}

.popup-enter, .popup-leave-to /* .popup-leave-active in <2.1.8 */ {
  opacity: 0;
}

.popup-enter-active .create-collection-popup-content,
.popup-leave-active .create-collection-popup-content {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.popup-enter .create-collection-popup-content,
.popup-leave-to .create-collection-popup-content {
  transform: translateY(-20px) scale(0.95);
  opacity: 0;
}

.popup-enter-to .create-collection-popup-content {
  transform: translateY(0) scale(1);
  opacity: 1;
}

.popup-title {
  font-size: 24px;
  color: #333;
  margin-bottom: 10px;
}

.collection-name-input {
  width: calc(100% - 20px);
  padding: 12px 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  outline: none;
  &:focus {
    border-color: #007bff;
    box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
  }
}

.popup-actions {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 10px;
}

.create-button, .cancel-button {
  padding: 12px 25px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.create-button {
  background-color: #007bff;
  color: white;
  &:hover {
    background-color: #0056b3;
    transform: translateY(-2px);
  }
}

.cancel-button {
  background-color: #f0f2f5;
  color: #333;
  &:hover {
    background-color: #e0e0e0;
    transform: translateY(-2px);
  }
}
</style> 