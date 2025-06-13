<template>
  <div class="page-main">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <BookPopup
      :is-open="isPopupOpen"
      :book="selectedBook"
      @close="closePopup"
    />
    <div class="main-page-container">
      <nuxt :key="$route.fullPath" />
      <!-- Верхний блок с популярными главами -->
      <h2 class="title-text">Обзор →</h2>
      <section class="popular">
        <div v-for="item in popularBooks" :key="item.id" class="main-book-card cursor-pointer" @click="openPopup(item)">
          <img :src="item.cover" :alt="item.title" />
          <div class="title">{{ item.title }}</div>
          <div class="chapter">{{ item.chapter }}</div>
        </div>
      </section>

      <!-- Блок "Сейчас читают" -->
      <section class="current-reads">
        <h2>Сейчас читают → </h2>
        <div class="reads-section">
          <!-- Новинки -->
          <div class="reads-category">
            <h3>Новинки</h3>
            <div v-for="item in currentReads.new" :key="item.id" class="read-item cursor-pointer" @click="openPopup(item)">
              <img :src="item.cover" :alt="item.title" />
              <div class="read-title">{{ item.title }}</div>
            </div>
          </div>
          <!-- Набирающее популярность -->
          <div class="reads-category">
            <h3>Набирающее популярность</h3>
            <div v-for="item in currentReads.trending" :key="item.id" class="read-item cursor-pointer" @click="openPopup(item)">
              <img :src="item.cover" :alt="item.title" />
              <div class="read-title">{{ item.title }}</div>
            </div>
          </div>
          <!-- Популярное -->
          <div class="reads-category">
            <h3>Популярное</h3>
            <div v-for="item in currentReads.popular" :key="item.id" class="read-item cursor-pointer" @click="openPopup(item)">
              <img :src="item.cover" :alt="item.title" />
              <div class="read-title">{{ item.title }}</div>
            </div>
          </div>
        </div>
      </section>

      <!-- Блок "Последние обновления" -->
      <section class="updates">
        <h2>Последние обновления</h2>
        <ul class="updates-list">
          <li v-for="book in updates" :key="book.id" class="update-item cursor-pointer" @click="openPopup(book)">
            <img :src="book.cover" :alt="book.title" class="book-image" />
            <div class="update-title">{{ book.title }}</div>
            <div class="update-chapter">{{ book.chapter }}</div>
          </li>
        </ul>

        
        <!-- <div class="update-item" v-for="update in updates" :key="update.id">
          <div class="update-title">{{ update.title }}</div>
          <div class="update-chapter">{{ update.chapter }}</div>
        </div> -->
      </section>
    </div>
  </div>
</template>

<script>
import BookPopup from '~/components/BookPopup.vue'

export default {
  name: 'IndexPage',
  components: {
    BookPopup
  },

  data() {
    return {
      isPopupOpen: false,
      selectedBook: null,
      popularBooks: [
        { id: 1, title: "Popular book 1", chapter: "Глава 181", cover: "book_pic6.webp" },
        { id: 2, title: "Popular book 2", chapter: "Глава 116", cover: "book_pic7.webp" },
        { id: 3, title: "Popular book 3", chapter: "Глава 415", cover: "book_pic.webp" },
        { id: 4, title: "Popular book 4", chapter: "Глава 356", cover: "book_pic5.webp" },
        { id: 5, title: "Popular book 5", chapter: "Глава 102", cover: "book_pic4.jpg" },
        { id: 6, title: "Popular book 6", chapter: "Глава 356", cover: "book_pic2.jpg" },
        { id: 7, title: "Popular book 7", chapter: "Глава 421", cover: "book_pic3.jpg" },
        { id: 8, title: "Popular book 8", chapter: "Глава 125", cover: "book_pic5.webp" },
        { id: 9, title: "Popular book 9", chapter: "Глава 950", cover: "book_pic.webp" },
        { id: 10, title: "Popular book 10", chapter: "Глава 753", cover: "book_pic4.jpg" },
        { id: 11, title: "Popular book 11", chapter: "Глава 654", cover: "book_pic5.webp" },
        { id: 12, title: "Popular book 12", chapter: "Глава 70", cover: "book_pic2.jpg" }
      ],
      currentReads: {
        new: [
          { id: 1, title: "Current reading new book 1", chapter: "Глава 181", cover: "book_pic.webp" },
          { id: 2, title: "Current reading new book 2", chapter: "Глава 116", cover: "book_pic2.jpg" },
          { id: 3, title: "Current reading new book 3", chapter: "Глава 807", cover: "book_pic.webp" },
          { id: 3, title: "Current reading new book 4", chapter: "Глава 807", cover: "book_pic.webp" }
        ],
        trending: [
          { id: 1, title: "Current trending book 1", chapter: "Глава 181", cover: "book_pic.webp" },
          { id: 2, title: "Current trending book 2", chapter: "Глава 116", cover: "book_pic2.jpg" },
          { id: 3, title: "Current trending book 3", chapter: "Глава 807", cover: "book_pic.webp" }
        ],
        popular: [
          { id: 1, title: "Current popular book 1", chapter: "Глава 181", cover: "book_pic.webp" },
          { id: 2, title: "Current popular book 2", chapter: "Глава 116", cover: "book_pic2.jpg" },
          { id: 3, title: "Сurrent popular book 3", chapter: "Глава 807", cover: "book_pic.webp" }
        ],
      },
      updates: [
        { id: 1, title: "Book 1", chapter: "Том 1 Глава 181", cover: "book_pic3.jpg" },
        { id: 2, title: "Book 2", chapter: "Том 2 Глава 165", cover: "book_pic6.webp" },
        { id: 3, title: "Book 3", chapter: "Глава 45", cover: "book_pic7.webp" },
        { id: 4, title: "Book 4", chapter: "Глава 85", cover: "book_pic5.webp" },
        { id: 5, title: "Book 5", chapter: "Том 1 Глава 105", cover: "book_pic2.jpg" }
      ],
    };
  },

  methods: {
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
    }
  }
}

</script>

<style lang="scss">
.title-text {
  transition: transform 0.3s ease;
}

.title-text:hover {
  transform: scale(1.02);
}

.main-page-container {
  max-width: 1200px;
  /* Ограничиваем максимальную ширину */
  margin: 0 auto;
  /* Центрируем страницу по горизонтали */
  padding: 20px;
  /* Отступы от краёв */
  box-sizing: border-box;
  /* Включаем padding в общую ширину */
  font-family: $main-font-family;
}

.page-container h2 {
  margin-top: 10px;
  font-size: $titleFontSize;
}

/* Стили для популярного блока */
.popular {
  margin-top: 20px;
  display: flex;
  gap: 15px;
  overflow-x: auto;
  white-space: nowrap;
  /* Предотвращает перенос элементов на новую строку */
  padding-bottom: 10px;
  /* Отступ для прокрутки */
}

.popular::-webkit-scrollbar {
  height: 8px;
  /* Высота полосы прокрутки */
}

.popular::-webkit-scrollbar-thumb {
  background-color: #555;
  /* Цвет бегунка */
  border-radius: 50px;
  /* Скругленные края */
}

.popular::-webkit-scrollbar-track {
  background-color: #333;
  /* Цвет фона полосы прокрутки */
}

.main-book-card {
  flex: 0 0 150px;
  text-align: center;
  transition: transform 0.3s ease;
}

.book-card img {
  width: 100%;
  border-radius: 5px;
  transition: transform 0.3s ease;
}

.book-card:hover img {
  transform: scale(1.02);
  /* Увеличение изображения на 2% */
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
}

.title {
  font-size: 14px;
  margin-top: 5px;
}

.chapter {
  font-size: 12px;
  color: #aaa;
}

/* "Сейчас читают" блок */
.current-reads h2 {
  margin-top: 40px;
}

.reads-section {
  margin-top: 20px;
  display: flex;
  gap: 20px;
  justify-content: space-between;
}

.reads-category {
  flex: 1;
}

.read-item {
  display: flex;
  align-items: center;
  margin: 10px 0;
  transition: transform 0.3s ease;
}

.read-item img {
  width: 50px;
  height: 70px;
  object-fit: cover;
  border-radius: 5px;
  margin-right: 10px;
  transition: transform 0.3s ease;
}

.read-item:hover img {
  transform: scale(1.1);
  /* Увеличение изображения на 10% */
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
}

.read-title {
  font-size: 14px;
  color: #202020;
}

.reads-category h3 {
  font-size: 16px;
}

.reads-category ul {
  list-style: none;
  padding: 0;
}

.reads-category li {
  margin: 5px 0;
  font-size: 14px;
}

/* Последние обновления */
.updates h2 {
  margin-top: 40px;
  font-size: $titleFontSize;
}

.updates-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.update-item {
  display: flex;
  align-items: center; /* Выравниваем по вертикали */
  margin-bottom: 15px;
  transition: transform 0.3s ease;
  margin: 10px 0;
}

.update-item:hover {
  transform: scale(1.05);
  /* Увеличение изображения на 10% */
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
}

.book-image {
  width: 50px; /* Ширина изображения */
  height: 75px; /* Высота изображения */
  object-fit: cover; /* Корректное отображение */
  border-radius: 5px; /* Скруглённые углы */
  margin-right: 15px; /* Отступ между изображением и текстом */
}

.update-title {
  font-size: 14px;
}

.update-chapter {
  margin-left: 5px;
  font-size: 12px;
  color: #aaa;
}
</style>