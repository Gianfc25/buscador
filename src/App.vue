<template>
  <div id="app">
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-succes">R&M</span>
          <span class="subtitle">Personajes</span>
        </h1>

        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input
              v-model="search"
              type="text"
              class="input is-rounded"
              v-on:keyup.enter="searchData"
            />
          </div>
          <div class="control">
            <button
              class="button is-success is-rounded"
              v-on:click="searchData"
            >
              Buscar
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div
        class="columns is-desktop is-mobile is-tablet is-multiline is-centered"
      >
        <character
          v-for="character of characters"
          v-bind:key="character.id"
          v-bind:character="character"
        />
      </div>
      <nav class="pagination" role="navigation" aria-level="pagination">
        <a class="pagination-previous" v-on:click="changePage(page - 1)"
          >Anterior</a
        >
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>
        <a class="pagination-next" v-on:click="changePage(page + 1)"
          >Siguiente</a
        >
      </nav>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Character from "./components/Character";

export default {
  name: "App",
  components: {
    Character, // character
  },
  data: function name(params) {
    return {
      characters: [],
      page: 1,
      pages: 1,
      search: "",
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search,
      };

      let result = axios
        .get("https://rickandmortyapi.com/api/character/", { params })
        .then((res) => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      this.page = 1;
      this.fetch();
    },
  },
};
</script>
