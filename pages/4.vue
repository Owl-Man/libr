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
          <div v-else v-for="book in filteredBooks" :key="updateKey" class="book-card flex mb-6">
            <img :src="book.image" :alt="book.title" class="w-20 h-50 rounded-lg mr-4 object-cover" />
            <div class="flex-1">
              <h3 class="text-black text-lg font-semibold">{{ book.title }}</h3>
              <p class="text-gray-600 text-sm">{{ book.chapters }} • {{ book.rating }} ★</p>
              <p class="text-gray-600 text-sm mt-1">{{ book.description }}</p>
              <button
                @click="addToBookmarks(book)"
                class="mt-2 p-1 bg-gray-200 text-gray-700 rounded-lg text-sm hover:bg-gray-300 transition"
              >
                {{ book.status }}
              </button>
            </div>
            <div class="flex items-center">
              <button class="p-2">
                <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z"></path>
                </svg>
              </button>
            </div>
          </div>
        </section>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'BookmarksPage',
  
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
              description: 'После окончания средней школы Нацуко Хиросе начинает карьеру аниматора. Ее талант быстро расцветает, и вскоре она...',
              image: 'book_pic.webp',
              status: 'Смотрю',
            },
            {
              id: 2,
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
      this.updateKey++
      },
      filterBooks() {
        // Логика фильтрации реализована в computed свойстве filteredBooks
      },
      addToBookmarks(book) {
        console.log(`Изменение статуса книги: ${book.title}`);
      },
    },
  };
  </script>
  
  <style lang="scss">
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
  
  /* Карточки книг */
  .book-card {
    transition: transform 0.3s ease;
  }
  
  .book-card:hover {
    transform: scale(1.02);
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .book-card img {
    transition: transform 0.3s ease;
    width: fit-content;
    max-width: 100px;
  }
  
  .book-card:hover img {
    transform: scale(1.05);
  }
  
  /* Стили для сообщения, если книг нет */
  .no-results {
    padding: 20px;
    font-size: 16px;
  }
  </style>