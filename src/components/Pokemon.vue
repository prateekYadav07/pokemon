<template>
  <div class="cards">
    <card-vue
      v-for="pokemon in pokemons"
      :key="pokemon.id"
      :class="{ opace: selectedId && pokemon.id !== selectedId }"
      class="card"
      @click="click(pokemon)"
    >
      <template v-slot:title>
        {{pokemon.id}} {{ pokemon.name }} 
      </template>

      <template v-slot:content>
        <img :src="pokemon.sprite" :alt="pokemon.name" />
      </template>

      <template v-slot:description>
        <div v-for="t in pokemon.types" :key="t">
          {{ t }}
        </div>
      </template>
    </card-vue>
  </div>
</template>

<script>
import CardVue from "./Card.vue";

export default {
  name: "PokemonVue",
  components: {
    CardVue,
  },

  props: {
    pokemons: {
      type: [],
      required: true,
    },

    selectedId: {
      type: Number,
    },
  },

  methods: {
    click(pokemon) {
      this.$emit("chosen", pokemon);
    },
  },
};
</script>

<style scoped>
.opace {
  opacity: 0.5;
}

.card:hover {
  opacity: 1;
}

.cards {
  display: flex;
  justify-content: left;
}

img {
  width: 100%;
}
</style>