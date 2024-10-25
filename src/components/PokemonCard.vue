<template>
    <div class="card pokemon-card">
      <img :src="pokemon.image" :class="{ 'blur': !uncovered }" class="card-img-top" alt="Pokemon">
      <div class="card-body text-center">
        <div v-if="!uncovered">
          <input v-model="selection" @keyup.enter="checkSelection" class="form-control mb-2" placeholder="Adivina el nombre">
          <button @click="checkSelection" class="btn btn-primary">Descubrir</button>
        </div>
        <div v-else>
          <p class="card-text">{{ pokemon.name.toUpperCase() }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    // Recibir el objeto Pokémon como prop
    props: ['pokemon'], 
    data() {
      return {
         // Almacena la suposición del usuario
        selection: '',   
        // Indica si el Pokémon ha sido descubierto     
        uncovered: false  
      };
    },
    methods: {
      checkSelection() {
        // Este if pasa ambos textos a minusculas, asi no hay problemas en como se ingrese el nombre
        if (this.selection.toLowerCase() === this.pokemon.name.toLowerCase()) {
          this.uncovered = true; // Revelar el Pokémon
          this.$emit('pokemon-uncovered'); // Emitir evento al padre
        } else {
          alert('Nombre incorrecto'); // Mensaje de error
        }
      }
    }
  };
  </script>
  
  <style>
  .pokemon-card {
    border: none; 
    border-radius: 0; 
    box-shadow: none; 
    text-align: center;
  }

  .card-text{
    font-weight: bold;
  }
  
  /* Efecto de desenfoque */
  .blur {
    filter: blur(5px) grayscale(100%);
  }
  </style>
  