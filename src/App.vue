<template>

  <pokemon-vue
    :pokemons="pokemons"
    :selectedId="selectedId"
    @chosen="fetchEvol"
  />

  <pokemon-vue :pokemons="evolutions" />
</template>

<script>
import PokemonVue from "./components/Pokemon.vue";

const api = "https://pokeapi.co/api/v2/pokemon";
let IDS = [];

export default {
  name: "App",
  components: {
    PokemonVue,
  },
  data() {
    return {
      pokemons: [],
      evolutions: [],
      selectedId: null,
    };
  },

  async created() {
    IDS = this.fillIds();
    this.pokemons = await this.fetchData(IDS);
  },

  methods: {
    fillIds() {
      const ids = [];
      for (let i = 1; i < 151; i++) {
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
