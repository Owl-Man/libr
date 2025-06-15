<template>
    <div class="page-main">
      <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
      <div class="page-container-4">
        <!-- Поиск -->
        <div class="search-bar flex items-center mb-6">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Поиск в «Закладках»"
            class="w-full p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            @input="filterBooks"
          />
          <button
            @click="filterBooks"
            class="ml-2 p-3 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition"
          >
            Найти
          </button>
        </div>
  
        <!-- Вкладки -->
        <div class="tabs flex gap-4 mb-6 border-b border-gray-200">
          <button
            v-for="tab in tabs"
            :key="tab"
            @click="switchTab(tab)"
            :class="[
              'pb-2 px-4 text-sm font-semibold',
              selectedTab === tab ? 'border-b-2 border-blue-500 text-blue-500' : 'text-gray-600'
            ]"
          >
            {{ tab }} ({{ (bookmarks[tab] || []).length }})
          </button>
        </div>
  
        <!-- Список книг -->
        <section class="bookmarks" :key="updateKey">
          <div v-if="filteredBooks.length === 0" class="no-results text-black text-center">
            <p>Книги не найдены.</p>
          </div>
          <div v-else class="bookmarks-grid">
            <div v-for="book in filteredBooks" :key="book.id" class="book-card cursor-pointer mb-6" @click="openPopup(book)">
              <img :src="book.image" :alt="book.title" />
              <div class="title">{{ book.title }}</div>
              <div class="chapter">{{ book.chapters }}</div>
              <div class="rating">{{ book.rating }} ★</div>
              <div class="author">{{ book.description }}</div>
              <button
                @click.stop="addToBookmarks(book)"
                class="mt-2 p-1 bg-gray-200 text-gray-700 rounded-lg text-sm hover:bg-gray-300 transition"
              >
                {{ book.status }}
              </button>
            </div>
          </div>
        </section>
      </div>
      <BookPopup
        :is-open="isPopupOpen"
        :book="selectedBook"
        @close="closePopup"
      />
    </div>
  </template>
  
  <script>
  import BookPopup from '~/components/BookPopup.vue'
  
  export default {
    name: 'BookmarksPage',
    components: {
      BookPopup
    },
  
    data() {
      return {
        updateKey: 0,
        searchQuery: '',
        selectedTab: 'История',
        tabs: ['История', 'Избранное', 'Смотрю', 'В планах', 'Просмотрено', 'Отложено', 'Брошено'],
        bookmarks: {
          История: [
            {
              id: 1,
              title: 'Полное исцеление',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'После окончания средней школы ... начинает карьеру аниматора. Ее талант быстро расцветает, и вскоре она...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 2,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic2.jpg',
              status: 'Смотрю',
            },
            {
              id: 20,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 21,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 22,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 23,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 24,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 25,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 26,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 27,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 28,
              title: 'Что-то еще',
              chapters: '11 из 12 гл',
              rating: 4.7,
              description: 'Что-то еще описание...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
          ],
          Избранное: [
            {
              id: 2,
              title: 'Что-то еще (2023)',
              chapters: '24 гл',
              rating: 4.8,
              description: 'Что-то еще...',
              image: 'book_pic2.jpg',
              status: 'В планах',
            },
          ],
          Смотрю: [
            {
              id: 3,
              title: 'Что-то еще 2: Восста...',
              chapters: '11 из 13 гл',
              rating: 4.9,
              description: 'Что-то еще...',
              image: 'book_pic3.jpg',
              status: 'Смотрю',
            },
          ],
          'В планах': [
            {
              id: 4,
              title: 'Книга 4',
              chapters: '20 гл',
              rating: 4.5,
              description: 'Описание книги 4...',
              image: 'book_pic4.jpg',
              status: 'В планах',
            },
            {
              id: 8,
              title: 'Книга 42',
              chapters: '20 гл',
              rating: 4.5,
              description: 'Описание книги 4...',
              image: 'book_pic4.jpg',
              status: 'В планах',
            },
          ],
          Просмотрено: [
            {
              id: 5,
              title: 'Книга 5',
              chapters: '15 гл',
              rating: 4.6,
              description: 'Описание книги 5...',
              image: 'book_pic5.webp',
              status: 'Просмотрено',
            },
            {
              id: 10,
              title: 'Книга 5',
              chapters: '15 гл',
              rating: 4.6,
              description: 'Описание книги 5...',
              image: 'book_pic5.webp',
              status: 'Просмотрено',
            },
            {
              id: 11,
              title: 'Книга 5',
              chapters: '15 гл',
              rating: 4.6,
              description: 'Описание книги 5...',
              image: 'book_pic5.webp',
              status: 'Просмотрено',
            },
            {
              id: 12,
              title: 'Книга 5',
              chapters: '15 гл',
              rating: 4.6,
              description: 'Описание книги 5...',
              image: 'book_pic5.webp',
              status: 'Просмотрено',
            },
          ],
          Отложено: [
            {
              id: 6,
              title: 'Книга 6',
              chapters: '10 гл',
              rating: 4.3,
              description: 'Описание книги 6...',
              image: 'book_pic6.webp',
              status: 'Отложено',
            },
          ],
          Брошено: [
            {
              id: 7,
              title: 'Книга 7',
              chapters: '5 гл',
              rating: 3.8,
              description: 'Описание книги 7...',
              image: 'book_pic7.webp',
              status: 'Брошено',
            },
          ],
        },
        isPopupOpen: false,
        selectedBook: null,
      };
    },
  
    computed: {
      filteredBooks() {
        // Получаем книги для текущей вкладки
        let books = this.bookmarks[this.selectedTab] || [];

        console.log(`Переключение на вкладку: ${this.selectedTab}`);
  
        // Применяем фильтр по поиску
        if (this.searchQuery) {
          const query = this.searchQuery.toLowerCase();
          books = books.filter(
            (book) =>
              book.title.toLowerCase().includes(query) ||
              book.description.toLowerCase().includes(query)
          );
        }
  
        return books;
      },
    },
  
    methods: {
      switchTab(tab) {
      this.selectedTab = tab;
      this.searchQuery = ''; // Сбрасываем поиск при переключении вкладки
      console.log(`Переключение на вкладку: ${tab}`);
      this.updateKey++;
      console.log(this.updateKey)
      },
      filterBooks() {
        
      },
      addToBookmarks(book) {
        console.log(`Изменение статуса книги: ${book.title}`);
      },
      openPopup(book) {
        this.selectedBook = {
          ...book,
          author: 'Автор ' + Math.floor(Math.random() * 10 + 1),
          genre: ['Фантастика', 'Роман', 'Детектив', 'Фэнтези', 'Научная литература'][Math.floor(Math.random() * 5)],
          country: ['Япония', 'Южная Корея', 'США', 'Россия', 'Франция'][Math.floor(Math.random() * 5)],
          pages: Math.floor(Math.random() * 500) + 100,
          year: Math.floor(Math.random() * 30) + 1990,
          rating: (Math.random() * 2 + 3).toFixed(1),
          description: 'Это увлекательная книга, которая погружает читателя в захватывающий мир приключений и открытий. История рассказывает о невероятных событиях, которые происходят с главными героями, и о том, как они справляются с различными испытаниями на своем пути.',
          ratings: {
            5: Math.floor(Math.random() * 100),
            4: Math.floor(Math.random() * 100),
            3: Math.floor(Math.random() * 100),
            2: Math.floor(Math.random() * 50),
            1: Math.floor(Math.random() * 20)
          }
        }
        this.isPopupOpen = true
      },
      closePopup() {
        this.isPopupOpen = false
        this.selectedBook = null
      },
    },
  };
  </script>
  
  <style lang="scss">
  @import "~assets/scss/_book-card.scss";
  .page-container-4 {
    max-width: 1200px;
    min-width: 800px;
    margin: 0 auto;
    padding: 20px;
    box-sizing: border-box;
    font-family: $main-font-family;
  }
  
  .page-container h2 {
    font-size: $titleFontSize;
  }
  
  .title-text {
    transition: transform 0.3s ease;
  }
  
  .title-text:hover {
    transform: scale(1.02);
  }
  
  /* Поиск */
  .search-bar {
    display: flex;
    align-items: center;
  }
  
  /* Вкладки */
  .tabs button {
    transition: color 0.3s ease, border-color 0.3s ease;
  }
  
  .tabs button:hover {
    color: #3b82f6;
  }
  
  /* Стили для сообщения, если книг нет */
  .no-results {
    padding: 20px;
    font-size: 16px;
  }
  
  .bookmarks-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
    gap: 28px 24px;
    margin-top: 10px;
  }
  </style>