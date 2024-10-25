<template>
  <div class="text-center mb-4">
    <img src="../assets/img/pokemon.png" alt="Pokémon" class="img-fluid">
    <h2 style="font-weight: bold;">¿Quién es ese Pokémon?</h2>
  </div>
  <div>
    <PokemonList />
  </div>
</template>

<script>
import PokemonList from '../components/PokemonList.vue';

export default {
  components: { PokemonList }
};
</script>

<style scoped>
/* Puedes agregar estilos adicionales aquí si es necesario */
</style>
