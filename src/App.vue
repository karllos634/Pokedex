<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokemon-logo.png" />
      <hr />
      <h4 class="is-size-4">Pokedex</h4>
      <input
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
        class="input is-rounded"/>
      <button
        class="button is-fullwidth is-success"
        id="buscaBtn"
        @click="buscar">
        Buscar
      </button>
    </div>
    <div class="column is-half is-offset-2">
      <div
        class="columns"
        v-for="pokemon in pokemonChunks"
        :key="pokemon.url">
        <div v-for="(poke, index) in pokemon" :key="poke.url">
          <div class="column">
            <pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";
import _ from "lodash";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegou a lista de pokemon");
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function () {
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.busca
        );
      }
    },
    novaLinha: function (index) {
      console.log(index % 4 == 1);
      return index % 4 == 1;
    },
  },
  computed: {
    pokemonChunks() {
      return _.chunk(Object.values(this.pokemons), 4);
    },
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

#buscaBtn {
  margin-top: 2%;
}
</style>