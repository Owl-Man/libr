<script>
import { ref, computed, watch } from '@nuxtjs/composition-api'; // Импорт из @nuxtjs/composition-api
import { useRouter, useRoute } from '@nuxtjs/composition-api'; // Импорт useRouter и useRoute

export default {
  name: 'CatalogPage',

  props: {
    initialQuery: {
      type: String,
      default: '',
    },
  },

  setup(props) {
    const searchQuery = ref(props.initialQuery);
    const selectedGenre = ref('');
    const selectedCountry = ref('');
    const selectedRating = ref('');
    const genres = ref(['Фантастика', 'Роман', 'Детектив', 'Фэнтези', 'Научная литература']);
    const countries = ref(['Япония', 'Южная Корея', 'США', 'Россия', 'Франция']);
    const categories = ref([
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
    ]);

    const router = useRouter();
    const route = useRoute();

    watch(() => route.value.params.query, (newQuery) => {
      searchQuery.value = newQuery || '';
    });

    const filteredCategories = computed(() => {
      let filtered = JSON.parse(JSON.stringify(categories.value));

      filtered = filtered.map((category) => {
        let filteredBooks = category.books;

        if (selectedGenre.value) {
          filteredBooks = filteredBooks.filter((book) => book.genre === selectedGenre.value);
        }

        if (selectedCountry.value) {
          filteredBooks = filteredBooks.filter((book) => book.country === selectedCountry.value);
        }

        if (selectedRating.value) {
          filteredBooks = filteredBooks.filter((book) => book.rating >= parseInt(selectedRating.value));
        }

        return {
          ...category,
          books: filteredBooks,
        };
      });

      filtered = filtered.filter((category) => category.books.length > 0);

      console.log('Отфильтрованные категории:', filtered);
      return filtered;
    });

    const searchResults = computed(() => {
      if (!searchQuery.value) return [];

      const query = searchQuery.value.toLowerCase();
      let allBooks = [];

      categories.value.forEach((category) => {
        allBooks = allBooks.concat(category.books);
      });

      let filteredBooks = allBooks.filter(
        (book) =>
          book.title.toLowerCase().includes(query) ||
          book.author.toLowerCase().includes(query)
      );

      if (selectedGenre.value) {
        filteredBooks = filteredBooks.filter((book) => book.genre === selectedGenre.value);
      }
      if (selectedCountry.value) {
        filteredBooks = filteredBooks.filter((book) => book.country === selectedCountry.value);
      }
      if (selectedRating.value) {
        filteredBooks = filteredBooks.filter((book) => book.rating >= parseInt(selectedRating.value));
      }

      console.log('Результаты поиска:', filteredBooks);
      return filteredBooks;
    });

    const performSearch = () => {
      if (searchQuery.value) {
        router.push(`/2/${encodeURIComponent(searchQuery.value)}`);
        setTimeout(() => {
          window.location.reload();
        }, 100);
      } else {
        router.push('/2');
        setTimeout(() => {
          window.location.reload();
        }, 100);
      }
    };

    const filterBooks = () => {
      console.log(`Фильтры изменились: жанр=${selectedGenre.value}, страна=${selectedCountry.value}, рейтинг=${selectedRating.value}`);
    };

    return {
      searchQuery,
      selectedGenre,
      selectedCountry,
      selectedRating,
      genres,
      countries,
      categories,
      filteredCategories,
      searchResults,
      performSearch,
      filterBooks,
    };
  },
};
</script>

  <style lang="scss">
  .page-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    box-sizing: border-box;
    font-family: $main-font-family;
    background-color: #fff;
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
  
  .book-card {
    transition: transform 0.3s ease;
  }
  
  .book-card img {
    transition: transform 0.3s ease;
  }
  
  .book-card:hover img {
    transform: scale(1.02);
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
  
  .rating {
    font-size: 12px;
  }
  
  /* Стили для результатов поиска */
  .search-results .book-card {
    width: fit-content;
    max-width: 600px;
  }
  
  .book-info {
    max-width: 400px;
  }
  
  /* Анимация появления/исчезновения */
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.3s ease;
  }
  
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
  
  /* Стили для сообщения, если книг не найдено */
  .no-results {
    text-align: center;
    color: #666;
    font-size: 16px;
  }
  </style>