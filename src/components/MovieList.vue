<template>
  <div>
    <div class="flex table-header">
      <div>Poster</div>
      <div @click="toggleTitleOrder()">
        Title
        <span class="arrow-up"></span>
        <span class="arrow-down"></span>
      </div>
      <div @click="toggleYearOrder()">
        Year
        <span class="arrow-up"></span>
        <span class="arrow-down"></span>
      </div>
    </div>
    <div v-for="mov in sortedMovies" class="flex row" :key="mov.imdbId">
      <div>
        <img :src="mov.Poster" class="poster" />
      </div>
      <div>{{ mov.Title }}</div>
      <div>{{ mov.Year }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      movies: [],
      titleOrder: "",
      yearOrder: "",
      sortKey: ""
    };
  },
  mounted() {
    fetch("https://www.omdbapi.com/?apikey=a567cf6c&s=love&type=movie")
      .then(res => res.json())
      .then(res => {
        this.movies = res.Search;
      })
      .catch();
  },
  computed: {
    sortedMovies() {
      if (this.sortKey === "title") {
        return [...this.movies].sort((a, b) =>
          this.titleOrder === "asc"
            ? a.Title.charCodeAt(0) - b.Title.charCodeAt(0)
            : b.Title.charCodeAt(0) - a.Title.charCodeAt(0)
        );
      }
      if (this.sortKey === "year") {
        return [...this.movies].sort((a, b) =>
          this.yearOrder === "asc" ? a.Year - b.Year : b.Year - a.Year
        );
      }
      return this.movies;
    }
  },
  methods: {
    toggleTitleOrder() {
      this.titleOrder = this.titleOrder === "asc" ? "dsc" : "asc";
      this.sortKey = "title";
    },
    toggleYearOrder() {
      this.yearOrder = this.yearOrder === "asc" ? "dsc" : "asc";
      this.sortKey = "year";
    }
  }
};
</script>

<style>
@import "./MovieList.css";
</style>
