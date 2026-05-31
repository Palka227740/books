<template>
  <div>
    <div class="container mt-4">
      <h1 class="text-center text-md-start">Книжный магазин</h1>

      <!-- Форма для добавления книги -->
      <div class="mb-4">
        <h4>Добавить новую книгу</h4>
        <form @submit.prevent="addBook" class="row g-2">
          <div class="col-12 col-md-6">
            <input
              v-model="newBook.title"
              placeholder="Название"
              class="form-control"
              required
            >
          </div>
          <div class="col-12 col-md-6">
            <input
              v-model="newBook.author"
              placeholder="Автор"
              class="form-control"
              required
            >
          </div>
          <div class="col-12 col-md-6">
            <input
              v-model.number="newBook.price"
              type="number"
              placeholder="Цена"
              class="form-control"
              required
            >
          </div>
          <div class="col-12 col-md-6">
            <input
              v-model="newBook.image"
              placeholder="Ссылка на изображение"
              class="form-control"
            >
          </div>
          <div class="col-12">
            <button type="submit" class="btn btn-success">Добавить</button>
          </div>
        </form>
      </div>

      <!-- Карточки книг -->
      <div class="row">
        <div
          class="col-12 col-md-6 col-lg-4"
          v-for="book in books"
          :key="book.id"
        >
          <div class="card mb-3">
            <img
              :src="book.image || 'https://via.placeholder.com/150'"
              class="card-img-top"
              alt="Обложка книги"
            >
            <div class="card-body">
              <h5 class="card-title">{{ book.title }}</h5>
              <p class="card-text">Автор: {{ book.author }}</p>
              <p class="card-text">Цена: {{ book.price }} руб.</p>
              <button
                @click="addToCart(book)"
                class="btn btn-primary"
              >
                В корзину
              </button>
              <button
                @click="removeBook(book.id)"
                class="btn btn-danger btn-sm ms-2"
              >
                Удалить
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Корзина -->
      <div v-if="cart.length > 0" class="alert alert-success">
        <h4>Корзина</h4>
        <ul class="list-group">
          <li
            v-for="item in cart"
            :key="item.id"
            class="list-group-item d-flex justify-content-between"
          >
            {{ item.title }} - {{ item.price }} руб.
            <button
              @click="removeFromCart(item.id)"
              class="btn btn-sm btn-danger"
            >
              Удалить
            </button>
          </li>
        </ul>
        <p><strong>Итого: {{ totalPrice }} руб.</strong></p>
      </div>

      <!-- Сообщение, если корзина пуста -->
      <div v-if="cart.length === 0" class="alert alert-info d-none d-md-block">
        Корзина пуста. Добавьте книги из каталога!
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      books: [
        {
          id: 1,
          title: 'Мастер и Маргарита',
          author: 'Михаил Булгаков',
          price: 500,
          image: 'https://via.placeholder.com/150'
        },
        {
          id: 2,
          title: '1984',
          author: 'Джордж Оруэлл',
          price: 400,
          image: 'https://via.placeholder.com/150'
        },
        {
          id: 3,
          title: 'Преступление и наказание',
          author: 'Фёдор Достоевский',
          price: 600,
          image: 'https://via.placeholder.com/150'
        },
        {
          id: 4,
          title: 'Война и мир',
          author: 'Лев Толстой',
          price: 800,
          image: 'https://via.placeholder.com/150'
        },
        {
          id: 5,
          title: 'Гарри Поттер',
          author: 'Джоан Роулинг',
          price: 700,
          image: 'https://via.placeholder.com/150'
        }
      ],
      cart: [],
      newBook: {
        id: null,
        title: '',
        author: '',
        price: null,
        image: ''
      }
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((sum, item) => sum + item.price, 0);
    }
  },
  methods: {
    addBook() {
      const newId = Math.max(...this.books.map(b => b.id)) + 1;
      const book = {
        id: newId,
        title: this.newBook.title,
        author: this.newBook.author,
        price: this.newBook.price,
        image: this.newBook.image
      };
      this.books.push(book);
      // Сброс формы
      this.newBook = {
        id: null,
        title: '',
        author: '',
        price: null,
        image: ''
      };
    },
    addToCart(book) {
      this.cart.push({ ...book });
    },
    removeFromCart(bookId) {
      this.cart = this.cart.filter(item => item.id !== bookId);
    },
    removeBook(bookId) {
      this.books = this.books.filter(book => book.id !== bookId);
      // Также удаляем из корзины, если книга там есть
      this.cart = this.cart.filter(item => item.id !== bookId);
    }
  }
};
</script>

<style>
/* Кастомные стили */
.card-title {
  font-weight: bold;
  color: #333;
}

.alert-success {
  background-color: #d4edda;
  border-color: #c3e6cb;
  color: #155724;
}

.alert-info {
  background-color: #d1ecf1;
  border-color: #bee5eb;
  color: #0c5460;
}

/* Адаптивные стили */
@media (max-width: 767.98px) {
  .card-body {
    text-align: center;
  }
}

@media (min-width: 992px) {
  .card {
    transition: transform 0.2s;
  }
  .card:hover {
    transform: scale(1.03);
  }
}
</style>
