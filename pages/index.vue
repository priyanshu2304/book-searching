<template>
  <div class="book-search">
    <h1 class="book-search__heading">Book Search</h1>
    <form class="book-search__form">
      <input class="book-search__input" type="text" placeholder="Search by title, author or genre" v-model="searchQuery">
      <button class="book-search__button" type="submit" @click.prevent="searchBooks">Search</button>
      <label class="book-search__label" for="sort">Sort by:</label>
      <select class="book-search__select" id="sort" v-model="sortOption" @change="sortBooks">
        <option class="book-search__option" value="title">Title</option>
        <option class="book-search__option" value="author">Author</option>
        <option class="book-search__option" value="genre">Genre</option>
      </select>
    </form>
    <div v-if="!filteredBooks.length" class="book-search__no-results">No results found</div>
    <ul class="book-search__list">
      <li class="book-search__item" v-for="book in filteredBooks" :key="book.title">
        <span class="book-search__title">{{ book.title }}</span> by
        <span class="book-search__author">{{ book.author }}</span> (
        <span class="book-search__genre">{{ book.genre }}</span>)
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
      books,
    }
  },
  computed: {
    filteredBooks() {
      return this.books.filter((book) => {
        const query = this.searchQuery.toLowerCase()
        const title = book.title.toLowerCase()
        const author = book.author.toLowerCase()
        const genre = book.genre.toLowerCase()
        return (
          title.includes(query) ||
          author.includes(query) ||
          genre.includes(query)
        )
      })
    },
  },
  methods: {
    searchBooks() {
      // You could implement additional logic here to handle the search functionality
      // For example, you could show a loading spinner while the search is being performed
    },
    sortBooks() {
      const option = this.sortOption
      this.books.sort((a, b) => a[option].localeCompare(b[option]))
    },
  },
}
</script>

<style lang="scss" scoped>
.book-search {
  &__heading {
    margin-bottom: 1rem;
    font-size: 2rem;
  }

  &__form {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
  }

  &__input {
    flex: 1;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 0.25rem;
  }

  &__button {
    margin-left: 1rem;
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 0.25rem;
    cursor: pointer;
  }

  &__label {
    margin-left: 1rem;
  }

  &__select {
    margin-left: 0.5rem;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 0.25rem;
    cursor: pointer;

    &::-ms-expand {
      display: none;
    }
  }

  &__option {
    padding: 0.5rem;
  }

  &__no-results {
    margin-top: 1rem;
    color: #dc3545;
  }

  &__list {
    margin-top: 1rem;
    list-style: none;
    padding: 0;
  }

  &__item {
    margin-bottom: 0.5rem;
  }

  &__title {
    font-weight: bold;
  }

  &__author {
    font-style: italic;
  }

  &__genre {
    text-transform: uppercase;
  }
}
</style>
