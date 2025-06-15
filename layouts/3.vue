<template>
    <div class="page-main">
      <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
      <div class="page-container">
        <!-- Заголовок и фильтр -->
        <div class="header-section flex justify-between items-center mb-6">
          <h2 class="title-text text-grey">Коллекции →</h2>
          <div class="filter-category">
            <select
              v-model="selectedFilter"
              @change="applyFilter"
              class="p-2 border rounded-lg bg-gray-800 text-white border-gray-600"
            >
              <option value="leaders">Лидеры рейтинга</option>
              <option value="year">Популярные за год</option>
              <option value="month">Популярные за месяц</option>
              <option value="week">Популярные за неделю</option>
              <option value="recent">Недавно добавленные</option>
            </select>
          </div>
        </div>
  
        <!-- Список коллекций -->
        <section class="collections">
          <div v-if="filteredCollections.length === 0" class="no-results text-white text-center">
            <p>Коллекции не найдены.</p>
          </div>
          <div v-else v-for="collection in filteredCollections" :key="collection.id" class="collection-card mb-6 cursor-pointer" @click="openCollectionPopup(collection)">
            <div class="collection-header flex items-center mb-2">
              <img :src="collection.image" :alt="collection.title" class="w-16 h-16 rounded-lg mr-4 text-gray" />
              <div>
                <h3 class="text-gray text-lg font-sm">{{ collection.title }}</h3>
                <p class="text-gray-400 text-sm">{{ collection.description }}</p>
              </div>
            </div>
            <div class="collection-stats flex items-center text-gray-400 text-sm">
              <span class="mr-4 flex items-center">
                <svg class="w-5 h-5 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path>
                </svg>
                {{ collection.comments }}
              </span>
              <span class="flex items-center">
                <svg class="w-5 h-5 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"></path>
                </svg>
                {{ collection.likes }}
              </span>
            </div>
          </div>
          <CollectionPopup
            :isOpen="isCollectionPopupOpen"
            :collection="selectedCollection"
            :onClose="closeCollectionPopup"
            @book-click="openBookPopup"
          />
          <BookPopup
            :is-open="isBookPopupOpen"
            :book="selectedBook"
            @close="closeBookPopup"
          />
        </section>
      </div>
    </div>
  </template>
  
  <script>
  import CollectionPopup from '~/components/CollectionPopup.vue'
  import BookPopup from '~/components/BookPopup.vue'
  export default {
    name: 'CollectionsPage',
  
    data() {
      return {
        selectedFilter: 'leaders', // Фильтр по умолчанию
        isCollectionPopupOpen: false,
        selectedCollection: null,
        isBookPopupOpen: false,
        selectedBook: null,
        collections: [
          {
            id: 1,
            title: 'Корги | Коллекция для принцессы дуги',
            description: 'Мини описание',
            image: 'book_pic.webp',
            books: [
              {
                title: 'Книга 1',
                cover: 'book_pic.webp',
                author: 'Автор 1',
                genre: 'Фантастика',
                country: 'Япония',
                pages: 320,
                year: 2018,
                rating: 4.7,
                description: 'Описание книги 1...',
                ratings: { 5: 53, 4: 4, 3: 14, 2: 11, 1: 9 }
              },
              {
                title: 'Книга 2',
                cover: 'book_pic2.jpg',
                author: 'Автор 2',
                genre: 'Роман',
                country: 'США',
                pages: 250,
                year: 2020,
                rating: 4.5,
                description: 'Описание книги 2...',
                ratings: { 5: 40, 4: 10, 3: 5, 2: 2, 1: 1 }
              },
              {
                title: 'Книга 3',
                cover: 'book_pic3.jpg',
                author: 'Автор 3',
                genre: 'Детектив',
                country: 'Россия',
                pages: 410,
                year: 2015,
                rating: 4.2,
                description: 'Описание книги 3...',
                ratings: { 5: 30, 4: 15, 3: 10, 2: 5, 1: 2 }
              },
            ],
            comments: 1,
            likes: 2,
            rating: 4.8,
            popularityYear: 500,
            popularityMonth: 200,
            popularityWeek: 50,
            createdAt: '2025-03-15',
          },
          {
            id: 2,
            title: 'Коллекция книг для зимы • уют',
            description: 'Мини описание',
            image: 'book_pic2.jpg',
            books: [
              { title: 'Книга 4', cover: 'book_pic4.jpg' },
              { title: 'Книга 5', cover: 'book_pic5.webp' },
            ],
            comments: 0,
            likes: 0,
            rating: 4.8,
            popularityYear: 300,
            popularityMonth: 150,
            popularityWeek: 30,
            createdAt: '2025-02-10',
          },
          {
            id: 3,
            title: 'Лучшее от ИмяАвтора',
            description: 'Мини описание',
            image: 'book_pic3.jpg',
            books: [
              { title: 'Книга 6', cover: 'book_pic6.webp' },
              { title: 'Книга 7', cover: 'book_pic7.webp' },
            ],
            comments: 7,
            likes: 0,
            rating: 4.6,
            popularityYear: 700,
            popularityMonth: 100,
            popularityWeek: 20,
            createdAt: '2024-11-01',
          },
          {
            id: 4,
            title: 'Мои любимые книги, где надо много думать, грустить и стоять у окна',
            description: 'Мини описание',
            image: 'book_pic4.jpg',
            books: [
              { title: 'Книга 8', cover: 'book_pic4.jpg' },
              { title: 'Книга 9', cover: 'book_pic.webp' },
            ],
            comments: 2,
            likes: 0,
            rating: 4.7,
            popularityYear: 400,
            popularityMonth: 120,
            popularityWeek: 80,
            createdAt: '2025-03-10',
          },
          {
            id: 5,
            title: 'Моя коллекция чего-то',
            description: 'Мини описание',
            image: 'book_pic4.jpg',
            comments: 2,
            likes: 0,
            rating: 4.7,
            popularityYear: 400,
            popularityMonth: 120,
            popularityWeek: 80,
            createdAt: '2025-03-10',
          },
          {
            id: 6,
            title: 'Моя коллекция чего-то 2',
            description: 'Мини описание',
            image: 'book_pic4.jpg',
            comments: 2,
            likes: 0,
            rating: 4.7,
            popularityYear: 400,
            popularityMonth: 120,
            popularityWeek: 80,
            createdAt: '2025-03-10',
          },
          {
            id: 7,
            title: 'Моя коллекция чего-то 3',
            description: 'Мини описание',
            image: 'book_pic4.jpg',
            comments: 2,
            likes: 0,
            rating: 4.7,
            popularityYear: 400,
            popularityMonth: 120,
            popularityWeek: 80,
            createdAt: '2025-03-10',
          },
          {
            id: 8,
            title: 'Моя коллекция чего-то 4',
            description: 'Мини описание',
            image: 'book_pic4.jpg',
            comments: 2,
            likes: 0,
            rating: 4.7,
            popularityYear: 400,
            popularityMonth: 120,
            popularityWeek: 80,
            createdAt: '2025-03-10',
          },
        ],
      };
    },
  
    computed: {
      filteredCollections() {
        let filtered = [...this.collections];
  
        if (this.selectedFilter === 'leaders') {
          filtered.sort((a, b) => b.rating - a.rating);
        } else if (this.selectedFilter === 'year') {
          filtered.sort((a, b) => b.popularityYear - a.popularityYear);
        } else if (this.selectedFilter === 'month') {
          filtered.sort((a, b) => b.popularityMonth - a.popularityMonth);
        } else if (this.selectedFilter === 'week') {
          filtered.sort((a, b) => b.popularityWeek - a.popularityWeek);
        } else if (this.selectedFilter === 'recent') {
          filtered.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
        }
  
        return filtered;
      },
    },
  
    methods: {
      applyFilter() {
        // Логика фильтрации реализована в computed свойстве filteredCollections
      },
      openCollectionPopup(collection) {
        this.selectedCollection = collection;
        this.isCollectionPopupOpen = true;
      },
      closeCollectionPopup() {
        this.isCollectionPopupOpen = false;
        this.selectedCollection = null;
      },
      openBookPopup(book) {
        this.selectedBook = {
          ...book,
          author: book.author || ('Автор ' + Math.floor(Math.random() * 10 + 1)),
          genre: book.genre || ['Фантастика', 'Роман', 'Детектив', 'Фэнтези', 'Научная литература'][Math.floor(Math.random() * 5)],
          country: book.country || ['Япония', 'Южная Корея', 'США', 'Россия', 'Франция'][Math.floor(Math.random() * 5)],
          pages: book.pages || (Math.floor(Math.random() * 500) + 100),
          year: book.year || (Math.floor(Math.random() * 30) + 1990),
          rating: book.rating || (Math.random() * 2 + 3).toFixed(1),
          description: book.description || 'Описание отсутствует',
          ratings: book.ratings || { 5: 0, 4: 0, 3: 0, 2: 0, 1: 0 }
        };
        this.isBookPopupOpen = true;
      },
      closeBookPopup() {
        this.isBookPopupOpen = false;
        this.selectedBook = null;
      },
    },
    components: {
      CollectionPopup,
      BookPopup
    },
  };
  </script>
  
  <style lang="scss">
  .page-container {
    max-width: 1500px;
    min-width: 1200px;
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
  
  .header-section {
    border-bottom: 1px solid #333;
    padding-bottom: 10px;
  }
  
  .filter-category select {
    transition: background-color 0.3s ease;
  }
  
  .filter-category select:hover {
    background-color: #444;
  }
  
  .collection-card {
    background-color: #bbd9e8;
    padding: 15px;
    border-radius: 8px;
    transition: transform 0.3s ease;
  }
  
  .collection-card:hover {
    transform: scale(1.02);
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
  }
  
  .collection-header img {
    transition: transform 0.3s ease;
  }
  
  .collection-header:hover img {
    transform: scale(1.05);
  }
  
  .collection-stats svg {
    transition: color 0.3s ease;
  }
  
  .collection-stats span:hover svg {
    color: #ff5555;
  }
  
  /* Стили для сообщения, если коллекций нет */
  .no-results {
    padding: 20px;
    font-size: 16px;
  }
  </style>