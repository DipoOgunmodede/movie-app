<template>
  <HelloWorld :msg="generatedSearchInstructions" />
  <form
    class="py-4"
    @submit.prevent="queryDatabase(typeOfQuery, queryTerm, query)"
  >
    <select v-model="queryTerm">
      <option value="person">Person</option>
      <option value="movie">Film</option>
    </select>
    <SearchBar v-model="query" />
    <button>Search</button>
  </form>

  <div v-if="queryTerm === 'person'">
    <div
      v-if="films.length > 0"
      class="grid grid-cols-1 gap-8 md:grid-cols-4 searchresults"
    >
      <SearchCard v-for="film in films" :key="film.id" :item="film" />
    </div>
  </div>
  <div v-if="queryTerm === 'movie'" class="searchresults">
    <h3>{{ films.original_title }}</h3>
    {{ films.overview }}
    {{ films.release_date }}
    <img :src="`https://image.tmdb.org/t/p/w300${films.poster_path}`" />
    <!-- refactor this to be a different card -->
    <div v-if="films.length > 0">
      <p>{{ films.original_title }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import HelloWorld from "./components/HelloWorld.vue";
import SearchBar from "./components/SearchBar.vue";
import SearchCard from "./components/SearchCard.vue";
export default {
  name: "App",
  data() {
    return {
      msg: "",
      query: "",
      queryResults: [],
      typeOfQuery: "search",
      queryTerm: "movie",
      ID: 0,
      films: [],
    };
  },
  methods: {
    queryDatabase(typeOfQuery, queryTerm, query) {
      console.log("API Request:", typeOfQuery, queryTerm, query);
      const urlRequest = axios.get(
        `https://api.themoviedb.org/3/${typeOfQuery}/${queryTerm}/`,
        {
          params: {
            api_key: "c92bac37a196e6559bcb667ecb49b1e1",
            query: query,
          },
        }
      );
      urlRequest.then((response) => {
        this.queryResults = response.data.results;
        this.ID = this.queryResults[0].id;
        axios
          .get(this.generateQueryUrlFromID, {
            params: {
              api_key: "c92bac37a196e6559bcb667ecb49b1e1",
            },
          })
          .then((response) => {
            if (this.queryTerm === "person") {
              this.films = response.data.cast;
            } else if (this.queryTerm === "movie") {
              this.films = response.data;
            }
          });
        console.log(this.films.title);
      });
    },
  },
  computed: {
    generatedSearchInstructions() {
      return this.queryTerm == "movie" ? "Find a film" : "Find an actor";
    },
    generateQueryUrlFromID() {
      if (this.queryTerm === "person") {
        return `https://api.themoviedb.org/3/person/${this.ID}/movie_credits`;
      } else {
        return `https://api.themoviedb.org/3/movie/${this.ID}`;
      }
    },
  },
  components: {
    HelloWorld,
    SearchBar,
    SearchCard,
  },
  mounted() {
    this.queryDatabase(this.typeOfQuery, this.queryTerm, this.query);
    console.log(this.films);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
