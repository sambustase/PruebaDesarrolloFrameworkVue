<template>
    <div class="pokemon-card">
      <img :src="imageUrl" :style="{ filter: isDiscovered ? 'none' : 'blur(5px)' }" alt="Pokémon" />
      <input v-if="!isDiscovered" v-model="guess" placeholder="Nombre del Pokémon" />
      <button v-if="!isDiscovered" @click="checkGuess">Descubrir</button>
    </div>
  </template>
  
  <script>
  export default {
    props: ['imageUrl', 'name'],
    data() {
      return {
        guess: '',
        isDiscovered: false,
      };
    },
    methods: {
      checkGuess() {
        if (this.guess.toLowerCase() === this.name.toLowerCase()) {
          this.isDiscovered = true;
          this.$emit('pokemon-discovered'); // Notificar al padre que un Pokémon fue descubierto
        } else {
          alert('¡Incorrecto! Inténtalo de nuevo.');
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .pokemon-card {
    margin: 10px;
    text-align: center;
  }
  
  img {
    max-width: 150px;
    max-height: 150px;
  }
  
  input {
    margin-top: 10px;
    padding: 5px;
    font-size: 14px;
  }
  
  button {
    margin-top: 5px;
    padding: 5px 10px;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #45a049;
  }
  </style>
  