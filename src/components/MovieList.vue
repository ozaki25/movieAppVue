<template>
  <div>
    <div class="flex table-header">
      <div>Poster</div>
      <div @click="sort('name')">
        Title
        <span class="arrow-up"></span>
        <span class="arrow-down"></span>
      </div>
      <div @click="sort('year')">
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
  data: () => ({
    movies: [],
    titleOrder: "asc",
    yearOrder: "asc"
  }),
  mounted() {
    fetch("https://www.omdbapi.com/?apikey=a567cf6c&s=love&type=movie")
      .then(res => res.json())
      .then(res => {
        this.movies = res.Search;
      })
      .catch();
  },
  methods: {
    sort(para) {
      const movies = this.movies;
      let titleOrder = this.titleOrder;
      let yearOrder = this.yearOrder;
      if (para == "name") {
        movies.sort((a, b) =>
          titleOrder === "asc"
            ? b.Title.charCodeAt(0) - a.Title.charCodeAt(0)
            : a.Title.charCodeAt(0) - b.Title.charCodeAt(0)
        );
        titleOrder = titleOrder == "asc" ? "dsc" : "asc";
      }
      if (para === "year") {
        movies.sort((a, b) =>
          yearOrder == "asc" ? a.Year - b.Year : b.Year - a.Year
        );
        yearOrder = yearOrder == "asc" ? "dsc" : "asc";
      }

      this.movies = movies;
      this.titleOrder = titleOrder;
      this.yearOrder = yearOrder;
    }
  }
};
</script>

<style>
@import "./MovieList.css";
</style>
