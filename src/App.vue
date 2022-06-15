<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <pokemon-vue
          :pokemons="pokemons"
          :selectedId="selectedId"
          @chosen="fetchEvol"
        />
      </div>
      <div class="col">
        <pokemon-vue :pokemons="evolutions" />
      </div>
    </div>
    <div class="row">
      <div class="col">
        <pokemon-vue
          :pokemons="pokemons1"
          :selectedId="selectedId"
          @chosen="fetchEvol"
        />
      </div>
    </div>
    <div class="row">
      <div class="col">
        <pokemon-vue
          :pokemons="pokemons2"
          :selectedId="selectedId"
          @chosen="fetchEvol"
        />
      </div>
    </div>
  </div>
</template>

<script>
import PokemonVue from "./components/Pokemon.vue";

const api = "https://pokeapi.co/api/v2/pokemon";
let IDS = [1, 4, 7];
let IDS1 = [10, 13, 16];
let IDS2 = [19, 29, 32];

export default {
  name: "App",
  components: {
    PokemonVue,
  },
  data() {
    return {
      pokemons: [],
      pokemons1: [],
      pokemons2: [],
      evolutions: [],
      selectedId: null,
    };
  },

  async created() {
    this.pokemons = await this.fetchData(IDS);
    this.pokemons1 = await this.fetchData(IDS1);
    this.pokemons2 = await this.fetchData(IDS2);
  },

  methods: {
    fillIds() {
      const ids = [];
      for (let i = 1; i < 18; i += 3) {
        ids.push(i);
      }
      return ids;
    },

    async fetchEvol(pokemon) {
      this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2]);
      this.selectedId = pokemon.id;
    },

    async fetchData(ids) {
      const responses = await Promise.all(
        ids.map((id) => window.fetch(`${api}/${id}`))
      );
      const json = await Promise.all(
        responses.map((response) => response.json())
      );
      return json.map((data) => ({
        id: data.id,
        name: data.name,
        sprite: data.sprites.other["official-artwork"].front_default,
        types: data.types.map((type) => type.type["name"]),
      }));
    },
  },
};
</script>

<style>
</style>
