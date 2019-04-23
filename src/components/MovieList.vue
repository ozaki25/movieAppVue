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
    <div v-for="mov in movies" class="flex row" :key="mov.imdbId">
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
      yearOrder: ""
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
  watch: {
    titleOrder(newProp) {
      this.movies = [...this.movies].sort((a, b) =>
        newProp === "asc"
          ? b.Title.charCodeAt(0) - a.Title.charCodeAt(0)
          : a.Title.charCodeAt(0) - b.Title.charCodeAt(0)
      );
    },
    yearOrder(newProp) {
      this.movies = [...this.movies].sort((a, b) =>
        newProp === "asc" ? b.Year - a.Year : a.Year - b.Year
      );
    }
  },
  methods: {
    toggleTitleOrder() {
      this.titleOrder = this.titleOrder === "asc" ? "dsc" : "asc";
    },
    toggleYearOrder() {
      this.yearOrder = this.yearOrder === "asc" ? "dsc" : "asc";
    }
  }
};
</script>

<style>
@import "./MovieList.css";
</style>
