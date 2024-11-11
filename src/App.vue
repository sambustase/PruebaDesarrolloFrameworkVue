<template>
  <div id="app">
    <h1>Adivina el Pokémon</h1>
    <div v-if="pokemonList.length">
      <h3>{{ discoveredCount }} de {{ pokemonList.length }} descubiertos</h3>
      <p>Progreso: {{ discoveryPercentage }}%</p>
      <div class="pokemon-container">
        <PokemonCard
          v-for="(pokemon, index) in pokemonList"
          :key="index"
          :imageUrl="pokemon.image"
          :name="pokemon.name"
          @pokemon-discovered="incrementCounter"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',
  components: { PokemonCard },
  data() {
    return {
      pokemonList: [], // Lista de pokemones
      discoveredCount: 0, // Contador de pokemones descubiertos
    };
  },
  computed: {
    discoveryPercentage() {
      return ((this.discoveredCount / this.pokemonList.length) * 100).toFixed(2);
    },
  },
  methods: {
    incrementCounter() {
      this.discoveredCount++;
    },
  },
  created() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=20')
      .then(response => {
        this.pokemonList = response.data.results.map(pokemon => ({
          name: pokemon.name,
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemon.url.split('/').slice(-2, -1)[0]}.png`,
        }));
      })
      .catch(error => {
        console.error('Error fetching Pokémon data:', error);
      });
  },
};
</script>

<style>
#app {
  text-align: center;
  font-family: Arial, sans-serif;
}

.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

h3 {
  font-size: 20px;
}

p {
  font-size: 18px;
}
</style>
