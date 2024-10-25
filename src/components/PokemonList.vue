<template>
  <div>
    <div class="counter">Pokémons descubiertos: {{ counter }}</div>
    <div class="container">
      <div class="row pokemon-list">
        <PokemonCard
          v-for="pokemon in pokemonData"
          :key="pokemon.id"
          :pokemon="pokemon"
          class="col-12 col-sm-6 col-md-4 col-lg-3 mb-4" 
          @pokemon-uncovered="increaseCounter"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './PokemonCard.vue';

export default {  
  
  components: { PokemonCard },
  data() {
    return {
      pokemons: [], // Almacena los datos crudos de los Pokémon
      counter: 0,
      limit: 20
    };
  },
  
  // Se carga la lista de Pokémones al montar el componente
  mounted() {
    this.listPokemons();
  },

  // Obtiene la lista de Pokémones y actualiza el contador de pokémones descubiertos
  computed: {
    pokemonData() {
      return this.pokemons.map((pokemon) => {
        return {
          name: pokemon.name,
          id: pokemon.id,
          image: pokemon.sprites.front_default,
        };
      });
    }
  },
  methods: {
    // Carga los Pokémon según el límite y actualiza el contador de pokémones descubiertos
    async loadPokemons() {
      const url = `https://pokeapi.co/api/v2/pokemon?offset=0&limit=${this.limit}`;
      const response = await axios.get(url);
      const pokemonData = response.data.results;

      // Mapea a promesas para obtener datos detallados de cada pokemon
      const pokemonPromises = pokemonData.map(async (pokemon) => {
        const pokeResponse = await axios.get(pokemon.url);
        return pokeResponse.data;
      });

      return await Promise.all(pokemonPromises);
    },

    // Llama la función para cargar los pokémones al montar el componente
    async listPokemons() {
      this.pokemons = await this.loadPokemons();
    },

    // Incrementa el contador de pokémones descubiertos
    increaseCounter() {
      this.counter += 1;
      // console.log('Contador incrementado: ', this.counter); verificaba el counter
    }
  }
};
</script>

<style>
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.counter {
  text-align: center;
  font-size: 1.5em;
  margin-bottom: 20px;
}
</style>
