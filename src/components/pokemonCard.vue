<template>
    <div class="pokemon-card">
      <h2>{{ name }}</h2>
      <img :src="image" :alt="name" class="pokemon-image"/>
      <div class="types">
        <span 
          v-for="type in types" 
          :key="type"
          :style="{ backgroundColor: typeColors[type] }"
          class="type-badge"
        >
          {{ type }}
        </span>
      </div>
      <div class="abilities">
        <span v-for="ability in abilities" :key="ability" class="ability-badge">
          {{ ability}}
        </span>
      </div>
    </div>
  </template>
  
  <script setup>
  import { computed } from 'vue'
  
  const props = defineProps({
    name: {
      type: String,
      required: true
    },
    image: {
      type: String,
      required: true
    },
    types: {
      type: Array,
      default: () => []
    },
    abilities: {
      type: Array,
      default: () => []
    }
  })
  
  const typeColors = {
    normal: "#A8A878",
    fire: "#F08030",
    water: "#6890F0",
    grass: "#78C850",
    electric: "#F8D030",
    ice: "#98D8D8",
    fighting: "#C03028",
    poison: "#A040A0",
    ground: "#E0C068",
    flying: "#A890F0",
    psychic: "#F85888",
    bug: "#A8B820",
    rock: "#B8A038",
    ghost: "#705898",
    dragon: "#7038F8",
    dark: "#705848",
    steel: "#B8B8D0",
    fairy: "#EE99AC"
  }
  </script>
  
  <style scoped>
  .pokemon-card {
    background-color: white;
    border: 1px solid #ccc;
    padding: 1rem;
    margin: 0.5rem;
    border-radius: 8px;
    text-align: center;
    transition: transform 0.2s;
  }
  
  .pokemon-card:hover {
    transform: translateY(-5px);
  }
  
  .pokemon-image {
    width: 120px;
    height: 120px;
    object-fit: contain;
  }
  
  .types {
    display: flex;
    gap: 0.5rem;
    justify-content: center;
    margin-top: 0.5rem;
  }
  
  .type-badge {
    padding: 0.25rem 0.75rem;
    border-radius: 16px;
    color: white;
    font-size: 0.9rem;
    text-transform: capitalize;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }
  .abilities {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
  margin-top: 0.5rem;
  flex-wrap: wrap;
}

.ability-badge {
  padding: 0.25rem 0.75rem;
  border-radius: 16px;
  background-color: #f0f0f0;
  font-size: 0.8rem;
  text-transform: capitalize;
}
  </style>