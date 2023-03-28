<template>
  <div class="book-search">
    <h1 class="book-search__heading">Book Search</h1>
    <form class="book-search__form">
      <input
        v-model="searchQuery"
        class="book-search__input"
        type="text"
        placeholder="Search by title, author or genre"
      />
      <button
        class="book-search__button"
        type="submit"
        @click.prevent="searchBooks($event)"
      >
        Search
      </button>
      <label class="book-search__label" for="sort">Sort by:</label>
      <select
        id="sort"
        v-model="sortOption"
        class="book-search__select"
        @change="sortBooks"
      >
        <option class="book-search__option" value="title">Title</option>
        <option class="book-search__option" value="author">Author</option>
        <option class="book-search__option" value="genre">Genre</option>
      </select>
    </form>
    <div v-if="!filteredBooks.length" class="book-search__no-results">
      No results found
    </div>
    <ul class="book-search__list">
      <li
        v-for="book in filteredBooks"
        class="book-search__item"
        :key="book.title"
      >
        <span class="book-search__title">{{ book.title }}</span> by
        <span class="book-search__author">{{ book.author }}</span>(
        <span class="book-search__genre">{{ book.genre }}</span
        >)
      </li>
    </ul>
  </div>
</template>

<script>
import { books } from './Constants'
export default {
  data() {
    return {
      searchQuery: '',
      sortOption: 'title',
      books: [],
    }
  },
  computed: {
    filteredBooks() {
      const searchRegex = new RegExp(this.searchQuery, 'i')
      return this.books.filter(
        (book) =>
          searchRegex.test(book.title) ||
          searchRegex.test(book.author) ||
          searchRegex.test(book.genre)
      )
    },
  },
  mounted() {
    this.books = books
  },
  methods: {
    searchBooks(event) {
      if (event.type !== 'click') {
        return
      }

      // Perform the search
      const searchRegex = new RegExp(this.searchQuery, 'i')
      this.filteredBooks = this.books.filter(
        (book) =>
          searchRegex.test(book.title) ||
          searchRegex.test(book.author) ||
          searchRegex.test(book.genre)
      )
    },
    sortBooks() {
      switch (this.sortOption) {
        case 'title':
          this.filteredBooks.sort((a, b) => a.title.localeCompare(b.title))
          break
        case 'author':
          this.filteredBooks.sort((a, b) => a.author.localeCompare(b.author))
          break
        case 'genre':
          this.filteredBooks.sort((a, b) => a.genre.localeCompare(b.genre))
          break
        default:
          break
      }
    },
  },
}
</script>

<style>
.book-search__heading {
  margin-bottom: 1rem;
  font-size: 2rem;
  display: flex;
  justify-content: center;
}

.book-search__form {
  display: flex;
  align-items: center;
  margin-bottom: 1rem;
}

.book-search__input {
  flex: 1;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.25rem;
}

.book-search__button {
  margin-left: 1rem;
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
}

.book-search__label {
  margin-left: 1rem;
}

.book-search__select {
  margin-left: 0.5rem;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.25rem;
  cursor: pointer;
}

.book-search__option {
  padding: 0.5rem;
}

.book-search__no-results {
  margin-top: 1rem;
  color: #dc3545;
}

.book-search__list {
  margin-top: 1rem;
  list-style: none;
  padding: 0;
}

.book-search__item {
  margin-bottom: 0.5rem;
}

.book-search__title {
  font-weight: bold;
}

.book-search__author {
  font-style: italic;
}

.book-search__genre {
  text-transform: uppercase;
}
</style>
