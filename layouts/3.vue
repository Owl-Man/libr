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
  
        <!-- Подвкладки и кнопка "Создать коллекцию" -->
        <div class="tabs-container flex justify-between items-center mb-6 border-b border-gray-200">
          <div class="tabs flex gap-4">
            <button
              @click="selectedCollectionTab = 'global'"
              :class="[
                'pb-2 px-4 text-sm font-semibold',
                selectedCollectionTab === 'global' ? 'border-b-2 border-blue-500 text-blue-500' : 'text-gray-600'
              ]"
            >
              Глобальные ({{ globalCollections.length }})
            </button>
            <button
              @click="selectedCollectionTab = 'saved'"
              :class="[
                'pb-2 px-4 text-sm font-semibold',
                selectedCollectionTab === 'saved' ? 'border-b-2 border-blue-500 text-blue-500' : 'text-gray-600'
              ]"
            >
              Сохраненные ({{ savedCollections.length }})
            </button>
          </div>
          <button
            @click="openCreateCollectionPopup"
            class="add-collection-button px-4 py-2 bg-green-500 text-white rounded-lg hover:bg-green-600 transition text-sm"
          >
            + Создать коллекцию
          </button>
        </div>
  
        <!-- Список коллекций -->
        <section class="collections">
          <div v-if="displayedCollections.length === 0" class="no-results text-white text-center">
            <p>Коллекции не найдены.</p>
          </div>
          <div v-else class="collections-grid">
            <div v-for="collection in displayedCollections" :key="collection.id" class="collection-card mb-6 cursor-pointer" @click="openCollectionPopup(collection)">
              <div class="collection-header flex items-center mb-2">
                <img :src="collection.image" :alt="collection.title" class="w-16 h-16 rounded-lg mr-4 text-gray" />
                <div>
                  <h3 class="text-gray text-lg font-sm">{{ collection.title }}</h3>
                  <p class="text-gray-400 text-sm collection-description">{{ collection.description }}</p>
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
              <div class="options-button" @click.stop="toggleCollectionMenu(collection.id)">
                <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z"></path>
                </svg>
                <div v-if="openMenuCollectionId === collection.id" class="options-menu" @click.stop>
                  <button 
                    v-if="!isCollectionSaved(collection.id)" 
                    @click="saveCollection(collection)"
                    class="option-item"
                  >
                    Сохранить
                  </button>
                  <button 
                    v-else 
                    @click="removeSavedCollection(collection.id)"
                    class="option-item"
                  >
                    Удалить
                  </button>
                </div>
              </div>
            </div>
          </div>
          <CollectionPopup
            :isOpen="isCollectionPopupOpen"
            :collection="selectedCollection"
            :onClose="closeCollectionPopup"
            @book-click="openBookPopup"
          />
          <BookPopup
            v-if="selectedBook"
            :is-open="isBookPopupOpen"
            :book="selectedBook"
            @close="closeBookPopup"
          />
          <CreateCollectionPopup
            :is-open="isCreateCollectionPopupOpen"
            @close="closeCreateCollectionPopup"
            @create="createCollection"
          />
        </section>
      </div>
    </div>
  </template>
  
  <script>
  import CollectionPopup from '~/components/CollectionPopup.vue'
  import BookPopup from '~/components/BookPopup.vue'
  import CreateCollectionPopup from '~/components/CreateCollectionPopup.vue'
  
  export default {
    name: 'CollectionsPage',
    components: {
      CollectionPopup,
      BookPopup,
      CreateCollectionPopup
    },
  
    data() {
      return {
        selectedFilter: 'leaders', // Фильтр по умолчанию
        isCollectionPopupOpen: false,
        selectedCollection: null,
        isBookPopupOpen: false,
        selectedBook: null,
        selectedCollectionTab: 'global', // Добавляем состояние для вкладок коллекций
        openMenuCollectionId: null, // Добавляем состояние для отслеживания открытого меню
        isCreateCollectionPopupOpen: false, // Added for CreateCollectionPopup
        globalCollections: [
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
                description: 'Описание книги 1... Это увлекательная книга, которая погружает читателя в захватывающий мир приключений и открытий.',
                ratings: { 5: 53, 4: 4, 3: 14, 2: 11, 1: 9 },
                chapters: '11 из 12 гл'
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
                description: 'Описание книги 2... История рассказывает о невероятных событиях, которые происходят с главными героями.',
                ratings: { 5: 40, 4: 10, 3: 5, 2: 2, 1: 1 },
                chapters: '20 из 25 гл'
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
                description: 'Описание книги 3... и о том, как они справляются с различными испытаниями на своем пути.',
                ratings: { 5: 30, 4: 15, 3: 10, 2: 5, 1: 2 },
                chapters: '5 из 10 гл'
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
              {
                title: 'Книга 4',
                cover: 'book_pic4.jpg',
                author: 'Автор 4',
                genre: 'Фантастика',
                country: 'Япония',
                pages: 300,
                year: 2022,
                rating: 4.0,
                description: 'Книга 4 описание.',
                chapters: '15 гл'
              },
              {
                title: 'Книга 5',
                cover: 'book_pic5.webp',
                author: 'Автор 5',
                genre: 'Роман',
                country: 'США',
                pages: 280,
                year: 2021,
                rating: 4.1,
                description: 'Книга 5 описание.',
                chapters: '12 гл'
              },
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
              {
                title: 'Книга 6',
                cover: 'book_pic6.webp',
                author: 'Автор 6',
                genre: 'Детектив',
                country: 'Россия',
                pages: 350,
                year: 2019,
                rating: 3.9,
                description: 'Книга 6 описание.',
                chapters: '18 гл'
              },
              {
                title: 'Книга 7',
                cover: 'book_pic7.webp',
                author: 'Автор 7',
                genre: 'Фантастика',
                country: 'Япония',
                pages: 400,
                year: 2023,
                rating: 4.8,
                description: 'Книга 7 описание.',
                chapters: '22 гл'
              },
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
              {
                title: 'Книга 8',
                cover: 'book_pic4.jpg',
                author: 'Автор 8',
                genre: 'Роман',
                country: 'США',
                pages: 220,
                year: 2017,
                rating: 4.3,
                description: 'Книга 8 описание.',
                chapters: '10 гл'
              },
              {
                title: 'Книга 9',
                cover: 'book_pic.webp',
                author: 'Автор 9',
                genre: 'Детектив',
                country: 'Россия',
                pages: 310,
                year: 2016,
                rating: 4.6,
                description: 'Книга 9 описание.',
                chapters: '14 гл'
              },
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
        savedCollections: [], // Saved collections will be stored here
      };
    },
  
    computed: {
      filteredCollections() {
        const collectionsToFilter = 
          this.selectedCollectionTab === 'global' ? this.globalCollections : this.savedCollections;

        let filtered = [...collectionsToFilter];
  
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

      displayedCollections() {
        return this.filteredCollections;
      }
    },
  
    methods: {
      applyFilter() {
        // Apply filter logic here
        console.log('Applying filter:', this.selectedFilter);
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
        this.selectedBook = book;
        this.isBookPopupOpen = true;
      },
      closeBookPopup() {
        this.isBookPopupOpen = false;
        this.selectedBook = null;
      },
      toggleCollectionMenu(collectionId) {
        this.openMenuCollectionId = this.openMenuCollectionId === collectionId ? null : collectionId;
      },
      isCollectionSaved(collectionId) {
        return this.savedCollections.some(collection => collection.id === collectionId);
      },
      saveCollection(collection) {
        if (!this.isCollectionSaved(collection.id)) {
          this.savedCollections.push(collection);
        }
        this.openMenuCollectionId = null;
      },
      removeSavedCollection(collectionId) {
        this.savedCollections = this.savedCollections.filter(collection => collection.id !== collectionId);
        this.openMenuCollectionId = null;
      },
      openCreateCollectionPopup() {
        this.isCreateCollectionPopupOpen = true;
      },
      closeCreateCollectionPopup() {
        this.isCreateCollectionPopupOpen = false;
      },
      createCollection(collectionName) {
        if (collectionName) {
          const newCollection = {
            id: Date.now(), // Simple unique ID
            title: collectionName,
            description: 'Новая коллекция',
            image: 'book_pic.webp', // Default image
            books: [],
            comments: 0,
            likes: 0,
            rating: 0,
            popularityYear: 0,
            popularityMonth: 0,
            popularityWeek: 0,
            createdAt: new Date().toISOString().split('T')[0],
          };
          this.globalCollections.push(newCollection);
        }
        this.closeCreateCollectionPopup();
      }
    },
    mounted() {
      document.addEventListener('click', this.handleClickOutside);
    },
    beforeDestroy() {
      document.removeEventListener('click', this.handleClickOutside);
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
  
  .collections-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
    margin-top: 20px;
    align-items: stretch;
  }

  .collection-card {
    background-color: #bbd9e8;
    padding: 15px;
    border-radius: 8px;
    transition: transform 0.3s ease;
    position: relative;
    min-height: 150px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    &:hover {
      transform: scale(1.02);
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
    }

    .collection-description {
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-overflow: ellipsis;
      margin-bottom: 10px;
    }

    .options-button {
      position: absolute;
      top: 10px;
      right: 10px;
      background: none;
      border: none;
      cursor: pointer;
      padding: 5px;
      border-radius: 50%;
      &:hover {
        background-color: rgba(0,0,0,0.05);
      }
      svg {
        display: block;
      }
    }

    .options-menu {
      position: absolute;
      top: 100%;
      right: 0;
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      z-index: 100;
      min-width: 120px;
      overflow: hidden;
    }

    .option-item {
      display: block;
      width: 100%;
      padding: 10px 15px;
      text-align: left;
      border: none;
      background: none;
      font-size: 14px;
      color: #333;
      cursor: pointer;
      &:hover {
        background-color: #f0f0f0;
      }
    }
  }

  /* Стили для вкладок */
  .tabs button {
    transition: color 0.3s ease, border-color 0.3s ease;
  }
  
  .tabs button:hover {
    color: #3b82f6;
  }

  .no-results {
    color: #666;
  }

  .add-collection-button {
    transition: background-color 0.3s ease, transform 0.2s ease;
    &:hover {
      transform: translateY(-2px);
    }
  }
  </style>