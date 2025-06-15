<template>
  <transition name="fade">
    <div v-if="isOpen" class="book-popup-overlay" @click="closePopup">
      <div class="book-popup" @click.stop>
        <button class="close-button" @click="closePopup">&times;</button>
        <div class="book-popup-content">
          <div class="book-cover">
            <img :src="book?.cover || book?.image" :alt="book?.title" />
          </div>
          <div class="book-details">
            <h2>{{ book?.title }}</h2>
            <div class="book-meta">
              <p><strong>Автор:</strong> {{ book?.author }}</p>
              <p><strong>Жанр:</strong> {{ book?.genre }}</p>
              <p><strong>Страна:</strong> {{ book?.country }}</p>
              <p><strong>Количество страниц:</strong> {{ book?.pages || 'Не указано' }}</p>
              <p><strong>Год издания:</strong> {{ book?.year || 'Не указано' }}</p>
            </div>
            <div class="book-rating">
              <h3>Рейтинг: {{ book?.rating }} ★</h3>
              <div class="rating-stars">
                <span 
                  v-for="star in 5" 
                  :key="star"
                  class="star"
                  :class="{ 'active': star <= userRating }"
                  @click="setRating(star)"
                  @mouseover="hoverRating = star"
                  @mouseleave="hoverRating = 0"
                >
                  ★
                </span>
                <span class="rating-text" v-if="userRating">Ваша оценка: {{ userRating }} ★</span>
              </div>
              <div class="rating-breakdown">
                <p>5 ★: {{ book?.ratings?.[5] || 0 }}</p>
                <p>4 ★: {{ book?.ratings?.[4] || 0 }}</p>
                <p>3 ★: {{ book?.ratings?.[3] || 0 }}</p>
                <p>2 ★: {{ book?.ratings?.[2] || 0 }}</p>
                <p>1 ★: {{ book?.ratings?.[1] || 0 }}</p>
              </div>
            </div>
            <div class="book-actions">
              <button class="read-button" @click="startReading">
                Читать
                <span class="read-icon">→</span>
              </button>
              <div class="bookmark-dropdown">
                <button class="bookmark-button" @click="toggleBookmarkDropdown">
                  Добавить в закладки
                  <span class="bookmark-icon">▼</span>
                </button>
                <div v-if="isBookmarkDropdownOpen" class="bookmark-menu">
                  <button 
                    v-for="status in bookmarkStatuses" 
                    :key="status"
                    class="bookmark-option"
                    @click="addToBookmark(status)"
                  >
                    {{ status }}
                  </button>
                </div>
              </div>
            </div>
            <div class="book-description">
              <h3>Описание</h3>
              <p>{{ book?.description || 'Описание отсутствует' }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'BookPopup',
  props: {
    isOpen: {
      type: Boolean,
      required: true
    },
    book: {
      type: Object,
      required: true,
      default: () => ({})
    }
  },
  data() {
    return {
      userRating: 0,
      hoverRating: 0,
      isBookmarkDropdownOpen: false,
      bookmarkStatuses: ['Избранное', 'Смотрю', 'В планах', 'Просмотрено', 'Отложено', 'Брошено']
    }
  },
  methods: {
    closePopup() {
      this.$emit('close')
    },
    setRating(rating) {
      this.userRating = rating
      // Здесь добавить логику сохранения оценки
    },
    startReading() {
      // Здесь добавить логику начала чтения
      console.log('Начало чтения книги:', this.book.title)
    },
    toggleBookmarkDropdown() {
      this.isBookmarkDropdownOpen = !this.isBookmarkDropdownOpen
    },
    addToBookmark(status) {
      console.log(`Добавление книги "${this.book.title}" в закладки: ${status}`)
      this.isBookmarkDropdownOpen = false
      // Здесь добавить логику сохранения в закладки
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

.book-popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 3000;
  backdrop-filter: blur(5px);
}

.book-popup {
  background-color: white;
  border-radius: 12px;
  padding: 20px 18px;
  max-width: 1400px;
  width: 98%;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  animation: popup-appear 0.3s ease-out;
}

@keyframes popup-appear {
  from {
    transform: scale(0.9);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.close-button {
  position: absolute;
  top: 20px;
  right: 20px;
  background: none;
  border: none;
  font-size: 32px;
  cursor: pointer;
  color: #666;
  transition: transform 0.2s ease;
  padding: 0;
  line-height: 1;
  &:hover {
    color: #000;
    transform: scale(1.2);
  }
}

.book-popup-content {
  display: flex;
  gap: 40px;
}

.book-cover {
  flex: 0 0 300px;
  img {
    width: 100%;
    border-radius: 12px;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease;
    &:hover {
      transform: scale(1.05);
    }
  }
}

.book-details {
  flex: 1;
  h2 {
    margin-top: 0;
    margin-bottom: 24px;
    font-size: 32px;
    color: #333;
  }
}

.book-meta {
  margin-bottom: 30px;
  p {
    margin: 12px 0;
    font-size: 16px;
    color: #666;
  }
}

.book-rating {
  margin-bottom: 30px;
  h3 {
    margin-bottom: 12px;
    font-size: 20px;
    color: #333;
  }
  .rating-stars {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 15px;
    .star {
      font-size: 32px;
      color: #ddd;
      cursor: pointer;
      transition: transform 0.2s ease, color 0.2s ease;
      &:hover {
        transform: scale(1.2);
      }
      &.active {
        color: #ffd700;
      }
    }
    .rating-text {
      margin-left: 15px;
      color: #666;
      font-size: 16px;
    }
  }
  .rating-breakdown {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 12px;
    p {
      margin: 0;
      color: #666;
      font-size: 14px;
    }
  }
}

.book-actions {
  display: flex;
  gap: 10px;
  margin: 18px 0 18px 0;
  align-items: center;
}

.bookmark-dropdown {
  position: relative;
  min-width: 0;
}

.bookmark-button {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 18px;
  background-color: #f0f0f0;
  color: #333;
  border: none;
  border-radius: 10px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.bookmark-menu {
  position: absolute;
  top: 100%;
  left: 0;
  margin-top: 6px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.13);
  overflow-y: auto;
  z-index: 1000;
  min-width: 160px;
  max-height: 220px;
  padding: 4px 0;
}

.bookmark-option {
  display: block;
  width: 100%;
  padding: 8px 18px;
  text-align: left;
  border: none;
  background: none;
  font-size: 15px;
  color: #333;
  cursor: pointer;
  transition: background-color 0.2s ease;

  &:hover {
    background-color: #f5f5f5;
  }
}

.book-description {
  margin-bottom: 30px;
  h3 {
    margin-bottom: 12px;
    font-size: 20px;
    color: #333;
  }
  p {
    line-height: 1.8;
    font-size: 16px;
    color: #444;
  }
}

.read-button {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 22px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin: 0;

  .read-icon {
    transition: transform 0.3s ease;
  }
  &:hover .read-icon {
    transform: translateX(5px);
  }
}

@media (max-width: 768px) {
  .book-popup-content {
    flex-direction: column;
  }
  
  .book-cover {
    flex: 0 0 auto;
    max-width: 180px;
    margin: 0 auto;
  }

  .rating-breakdown {
    grid-template-columns: repeat(2, 1fr) !important;
  }

  .book-popup {
    max-width: 99vw;
    padding: 10px 2vw;
  }

  .book-actions {
    flex-direction: column;
    gap: 8px;
    margin: 12px 0;
  }
}
</style> 