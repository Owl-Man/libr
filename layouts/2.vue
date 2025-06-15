<template>
  <div class="page-main">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <div class="page-container">
      <BookPopup
        :is-open="isPopupOpen"
        :book="selectedBook"
        @close="closePopup"
      />
      <!-- Поиск -->
      <div class="search-bar">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Поиск книг, авторов..."
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

      <!-- Фильтры -->
      <section class="filters">
        <h2 class="title-text">Фильтры →</h2>
        <div class="filter-section flex gap-6 mt-4">
          <div class="filter-category">
            <h3>Жанр</h3>
            <select
              v-model="selectedGenre"
              @change="filterBooks"
              class="p-2 border rounded-lg w-full mt-2"
            >
              <option value="">Все жанры</option>
              <option v-for="genre in genres" :key="genre" :value="genre">
                {{ genre }}
              </option>
            </select>
          </div>
          <div class="filter-category">
            <h3>Страна</h3>
            <select
              v-model="selectedCountry"
              @change="filterBooks"
              class="p-2 border rounded-lg w-full mt-2"
            >
              <option value="">Все страны</option>
              <option v-for="country in countries" :key="country" :value="country">
                {{ country }}
              </option>
            </select>
          </div>
          <div class="filter-category">
            <h3>Рейтинг</h3>
            <select
              v-model="selectedRating"
              @change="filterBooks"
              class="p-2 border rounded-lg w-full mt-2"
            >
              <option value="">Любой рейтинг</option>
              <option value="5">5 звезд</option>
              <option value="4">4 звезды и выше</option>
              <option value="3">3 звезды и выше</option>
            </select>
          </div>
        </div>
      </section>

      <!-- Категории книг -->
      <section class="categories">
        <h2 class="title-text">Категории →</h2>
        <div v-for="category in filteredCategories" :key="category.name" class="category-section mt-6">
          <h3 class="text-lg font-semibold">{{ category.name }}</h3>
          <div class="book-list flex gap-4 overflow-x-auto mt-4">
            <div
              v-for="book in category.books"
              :key="book.title"
              class="book-card cursor-pointer"
              @click="openPopup(book)"
            >
              <img :src="book.cover" :alt="book.title" />
              <div class="title">{{ book.title }}</div>
              <div class="author">{{ book.author }}</div>
              <div class="rating">Рейтинг: {{ book.rating }} ★</div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import BookPopup from '~/components/BookPopup.vue'

export default {
  name: 'CatalogPage',
  components: {
    BookPopup
  },

  data() {
    return {
      searchQuery: '',
      selectedGenre: '',
      selectedCountry: '',
      selectedRating: '',
      genres: ['Фантастика', 'Роман', 'Детектив', 'Фэнтези', 'Научная литература'],
      countries: ['Япония', 'Южная Корея', 'США', 'Россия', 'Франция'], // Список стран
      categories: [
        {
          name: 'Фантастика',
          books: [
            { title: 'Книга 1', author: 'Автор 1', rating: 5, cover: 'book_pic.webp', genre: 'Фантастика', country: 'Япония' },
            { title: 'Книга 2', author: 'Автор 2', rating: 4, cover: 'book_pic2.jpg', genre: 'Фантастика', country: 'США' },
            { title: 'Книга 3', author: 'Автор 3', rating: 3, cover: 'book_pic3.jpg', genre: 'Фантастика', country: 'Россия' },
          ],
        },
        {
          name: 'Роман',
          books: [
            { title: 'Книга 4', author: 'Автор 1', rating: 5, cover: 'book_pic4.jpg', genre: 'Роман', country: 'Франция' },
            { title: 'Книга 5', author: 'Автор 4', rating: 4, cover: 'book_pic5.webp', genre: 'Роман', country: 'Южная Корея' },
          ],
        },
        {
          name: 'Детектив',
          books: [
            { title: 'Книга 6', author: 'Автор 2', rating: 4, cover: 'book_pic6.webp', genre: 'Детектив', country: 'США' },
            { title: 'Книга 7', author: 'Автор 3', rating: 3, cover: 'book_pic7.webp', genre: 'Детектив', country: 'Япония' },
          ],
        },
      ],
      isPopupOpen: false,
      selectedBook: null,
    };
  },

  computed: {
    filteredCategories() {
      let filtered = this.categories;

      // Фильтр по жанру
      if (this.selectedGenre) {
        filtered = filtered.filter((category) =>
          category.books.some((book) => book.genre === this.selectedGenre)
        );
      }

      // Фильтр по стране (заменили фильтр по автору)
      if (this.selectedCountry) {
        filtered = filtered.map((category) => ({
          ...category,
          books: category.books.filter((book) => book.country === this.selectedCountry),
        })).filter((category) => category.books.length > 0);
      }

      // Фильтр по рейтингу
      if (this.selectedRating) {
        filtered = filtered.map((category) => ({
          ...category,
          books: category.books.filter((book) => book.rating >= parseInt(this.selectedRating)),
        })).filter((category) => category.books.length > 0);
      }

      // Фильтр по поиску
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase();
        filtered = filtered.map((category) => ({
          ...category,
          books: category.books.filter(
            (book) =>
              book.title.toLowerCase().includes(query) ||
              book.author.toLowerCase().includes(query)
          ),
        })).filter((category) => category.books.length > 0);
      }

      return filtered;
    },
  },

  methods: {
    filterBooks() {
      // Логика фильтрации реализована в computed свойстве filteredCategories
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

.page-container {
  max-width: 1200px;
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
  margin-bottom: 20px;
}

/* Фильтры */
.filters h2 {
  margin-top: 20px;
}

.filter-section {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
}

.filter-category h3 {
  font-size: 16px;
}

/* Категории */
.categories h2 {
  margin-top: 40px;
}

.book-list {
  padding-bottom: 10px;
}

.book-list::-webkit-scrollbar {
  height: 8px;
}

.book-list::-webkit-scrollbar-thumb {
  background-color: #555;
  border-radius: 50px;
}

.book-list::-webkit-scrollbar-track {
  background-color: #333;
}
</style>