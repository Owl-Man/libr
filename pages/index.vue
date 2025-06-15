<template>
  <div class="page-main">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <BookPopup
      v-if="selectedBook"
      :is-open="isPopupOpen"
      :book="selectedBook"
      @close="closePopup"
      :user-collections="globalCollections"
      @add-book-to-collection="handleAddBookToCollection"
    />
    <div class="main-page-container">
      <!-- Верхний блок с популярными главами -->
      <h2 class="title-text">Обзор →</h2>
      <section class="popular">
        <div v-for="item in popularBooks" :key="item.id" class="book-card cursor-pointer" @click="openPopup(item)">
          <img :src="item.cover" :alt="item.title" />
          <div class="title">{{ item.title }}</div>
          <div class="author">{{ item.author }}</div>
          <div class="rating">{{ item.rating }} ★</div>
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
              <img :src="item.cover" :alt="item.title" class="read-item-image" />
              <div class="read-details">
                <div class="read-title">{{ item.title.replace(/\s*Автор\s*\d+/g, '') }}</div>
                <div class="read-rating">{{ item.rating }} ★</div>
              </div>
            </div>
          </div>
          <!-- Набирающее популярность -->
          <div class="reads-category">
            <h3>Набирающее популярность</h3>
            <div v-for="item in currentReads.trending" :key="item.id" class="read-item cursor-pointer" @click="openPopup(item)">
              <img :src="item.cover" :alt="item.title" class="read-item-image" />
              <div class="read-details">
                <div class="read-title">{{ item.title.replace(/\s*Автор\s*\d+/g, '') }}</div>
                <div class="read-rating">{{ item.rating }} ★</div>
              </div>
            </div>
          </div>
          <!-- Популярное -->
          <div class="reads-category">
            <h3>Популярное</h3>
            <div v-for="item in currentReads.popular" :key="item.id" class="read-item cursor-pointer" @click="openPopup(item)">
              <img :src="item.cover" :alt="item.title" class="read-item-image" />
              <div class="read-details">
                <div class="read-title">{{ item.title.replace(/\s*Автор\s*\d+/g, '') }}</div>
                <div class="read-rating">{{ item.rating }} ★</div>
              </div>
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
            <div class="update-details">
              <div class="update-title">{{ book.title.replace(/\s*Автор\s*\d+/g, '') }}</div>
              <div class="update-rating">{{ book.rating }} ★</div>
            <div class="update-chapter">{{ book.chapter }}</div>
            </div>
          </li>
        </ul>
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
      globalCollections: [
        {
          id: 1,
          title: 'Корги | Коллекция для принцессы дуги',
          description: 'Мини описание',
          image: 'book_pic.webp',
          books: [],
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
          books: [],
          comments: 0,
          likes: 1,
          rating: 4.0,
          popularityYear: 300,
          popularityMonth: 100,
          popularityWeek: 30,
          createdAt: '2025-03-10',
        },
      ],
      popularBooks: [
        { id: 1, title: "Popular book 1", chapter: "Глава 181", cover: "book_pic6.webp", author: "Автор 1", rating: 4.5 },
        { id: 2, title: "Popular book 2", chapter: "Глава 116", cover: "book_pic7.webp", author: "Автор 2", rating: 4.2 },
        { id: 3, title: "Popular book 3", chapter: "Глава 415", cover: "book_pic.webp", author: "Автор 3", rating: 4.8 },
        { id: 4, title: "Popular book 4", chapter: "Глава 356", cover: "book_pic5.webp", author: "Автор 4", rating: 3.9 },
        { id: 5, title: "Popular book 5", chapter: "Глава 102", cover: "book_pic4.jpg", author: "Автор 5", rating: 4.1 },
        { id: 6, title: "Popular book 6", chapter: "Глава 356", cover: "book_pic2.jpg", author: "Автор 6", rating: 4.6 },
        { id: 7, title: "Popular book 7", chapter: "Глава 421", cover: "book_pic3.jpg", author: "Автор 7", rating: 4.0 },
        { id: 8, title: "Popular book 8", chapter: "Глава 125", cover: "book_pic5.webp", author: "Автор 8", rating: 4.3 },
        { id: 9, title: "Popular book 9", chapter: "Глава 950", cover: "book_pic.webp", author: "Автор 9", rating: 4.7 },
        { id: 10, title: "Popular book 10", chapter: "Глава 753", cover: "book_pic4.jpg", author: "Автор 10", rating: 4.4 },
        { id: 11, title: "Popular book 11", chapter: "Глава 654", cover: "book_pic5.webp", author: "Автор 11", rating: 4.9 },
        { id: 12, title: "Popular book 12", chapter: "Глава 70", cover: "book_pic2.jpg", author: "Автор 12", rating: 3.5 }
      ],
      currentReads: {
        new: [
          { id: 1, title: "Current reading new book 1", chapter: "Глава 181", cover: "book_pic.webp", author: "Автор 1", rating: 4.7  },
          { id: 2, title: "Current reading new book 2", chapter: "Глава 116", cover: "book_pic2.jpg", author: "Автор 2", rating: 4.2 },
          { id: 3, title: "Current reading new book 3", chapter: "Глава 807", cover: "book_pic.webp", author: "Автор 3", rating: 4.0 },
          { id: 4, title: "Current reading new book 4", chapter: "Глава 807", cover: "book_pic.webp", author: "Автор 4", rating: 4.1 }
        ],
        trending: [
          { id: 1, title: "Current trending book 1", chapter: "Глава 181", cover: "book_pic.webp", author: "Автор 5", rating: 4.9 },
          { id: 2, title: "Current trending book 2", chapter: "Глава 116", cover: "book_pic2.jpg", author: "Автор 6", rating: 3.8 },
          { id: 3, title: "Current trending book 3", chapter: "Глава 807", cover: "book_pic.webp", author: "Автор 7", rating: 4.4 }
        ],
        popular: [
          { id: 1, title: "Current popular book 1", chapter: "Глава 181", cover: "book_pic.webp", author: "Автор 8", rating: 4.5 },
          { id: 2, title: "Current popular book 2", chapter: "Глава 116", cover: "book_pic2.jpg", author: "Автор 9", rating: 4.0 },
          { id: 3, title: "Сurrent popular book 3", chapter: "Глава 807", cover: "book_pic.webp", author: "Автор 10", rating: 4.3 }
        ],
      },
      updates: [
        { id: 1, title: "Book 1", chapter: "Том 1 Глава 181", cover: "book_pic3.jpg", author: "Автор 1", rating: 4.7  },
        { id: 2, title: "Book 2", chapter: "Том 2 Глава 165", cover: "book_pic6.webp", author: "Автор 2", rating: 4.2 },
        { id: 3, title: "Book 3", chapter: "Глава 45", cover: "book_pic7.webp", author: "Автор 3", rating: 3.9 },
        { id: 4, title: "Book 4", chapter: "Глава 85", cover: "book_pic5.webp", author: "Автор 4", rating: 4.1 },
        { id: 5, title: "Book 5", chapter: "Том 1 Глава 105", cover: "book_pic2.jpg", author: "Автор 5", rating: 4.8 }
      ],
    };
  },

  methods: {
    openPopup(book) {
      this.selectedBook = {
        ...book,
        author: book.author || ('Автор ' + Math.floor(Math.random() * 10 + 1)),
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
    handleAddBookToCollection({ book, collectionId }) {
      const collection = this.globalCollections.find(col => col.id === collectionId);
      if (collection) {
        const bookExists = collection.books.some(b => b.title === book.title);
        if (!bookExists) {
          collection.books.push(book);
          console.log(`Книга "${book.title}" добавлена в коллекцию "${collection.title}"`);
        } else {
          console.log(`Книга "${book.title}" уже есть в коллекции "${collection.title}"`);
        }
      } else {
        console.error(`Коллекция с ID ${collectionId} не найдена.`);
      }
      this.closePopup();
    }
  }
}

</script>

<style lang="scss">
@import "~assets/scss/_book-card.scss";

.title-text {
  transition: transform 0.3s ease;
}

.title-text:hover {
  transform: scale(1.02);
}

.main-page-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  box-sizing: border-box;
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
  padding-bottom: 10px;
}

.popular::-webkit-scrollbar {
  height: 8px;
}

.popular::-webkit-scrollbar-thumb {
  background-color: #555;
  border-radius: 50px;
}

.popular::-webkit-scrollbar-track {
  background-color: #333;
}

/* "Сейчас читают" блок */
.current-reads h2 {
  margin-top: 40px;
}

.reads-section {
  margin-top: 20px;
  display: flex;
  gap: 20px;
}

.reads-category {
  flex: 1;
}

.read-item {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
  transition: transform 0.3s ease;
}

.read-item-image {
  width: 50px;
  height: 70px;
  object-fit: cover;
  border-radius: 5px;
  margin-right: 10px;
  transition: transform 0.3s ease;
  flex-shrink: 0;
}

.read-item:hover .read-item-image {
  transform: scale(1.1);
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
}

.read-details {
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-grow: 1;
  gap: 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.read-title {
  font-size: 14px;
  color: #202020;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  min-width: 0;
}

.read-rating {
  font-size: 12px;
  color: #ffc107;
  white-space: nowrap;
  flex-shrink: 0;
}

.reads-category h3 {
  font-size: 16px;
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
  align-items: center;
  margin-bottom: 15px;
  transition: transform 0.3s ease;
}

.book-image {
  width: 50px;
  height: 75px;
  object-fit: cover;
  border-radius: 5px;
  margin-right: 15px;
  flex-shrink: 0;
}

.update-details {
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-grow: 1;
  gap: 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.update-title {
  font-size: 14px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  min-width: 0;
}

.update-rating {
  font-size: 12px;
  color: #ffc107;
  white-space: nowrap;
  flex-shrink: 0;
}

.update-chapter {
  margin-left: 5px;
  font-size: 12px;
  color: #aaa;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  flex-shrink: 0;
}
</style>