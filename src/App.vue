<template>
  <div class="container m-auto text-center">
    <img class="w-25" alt="img_pokemon" src="./assets/logo2.png" />
    <h2>PokeGuía</h2>

    <div class="my-3">
      <input
        type="text"
        class="form-control w-25 m-auto"
        v-model="pokemoName"
        placeholder="Busca tu Pokémon"
        @keyup.enter="pokemonSearch"
      />
    </div>
    <button type="button" @click="pokemonSearch" class="btn btn-warning border border-primary border-5">
      Buscar
    </button>
  </div>

  <div class="container my-5 text-center d-flex bg-warning border border-primary border-5">
    <div class="w-50">
      <h4 class="text-uppercase">{{ nombre }}</h4>
      <img class="border" alt="pokemon" :src="imagen" />
    </div>

    <ul class="w-50">
      <h4 class="text-uppercase">Habilidades</h4>
      <li v-for="(habilidad, index) of habilidades" :key="index">
        {{ habilidad.ability.name }}
      </li>
    </ul>

    <ul class="w-50">
      <h4 class="text-uppercase">Movimientos</h4>
      <li v-for="(movimiento, index) of movimientos" :key="index">
        {{ movimiento.move.name }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      pokemoName: "",
      pokemonData: {
        name: "",
        abilities: [],
        moves: [],
      },
    };
  },

  created() {
    this.pokemonSearch();
  },

  methods: {
    async pokemonSearch() {
      try {
        const response = await fetch(this.url);
        const data = await response.json();
        this.pokemonData = data;
      } catch (error) {
        console.log(`Error: ${error}`);
      }
    },
  },

  computed: {
    url() {
      return this.pokemoName == ""
        ? `${this.baseUrl}pikachu`
        : `${this.baseUrl}${this.pokemoName}`;
    },
    baseUrl() {
      return "https://pokeapi.co/api/v2/pokemon/";
    },
    nombre() {
      return this.pokemonData.name;
    },
    habilidades() {
      return this.pokemonData.abilities;
    },
    movimientos() {
      return this.pokemonData.moves.slice(0, 10);
    },
    imagen() {
      return this.pokemonData.sprites.back_default;
    },
  },
};
</script>

<style>
ul {
  list-style: none;
}

img {
  /* width: 10%; */
}
</style>