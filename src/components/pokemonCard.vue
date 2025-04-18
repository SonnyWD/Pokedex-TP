<template>
    <div class="pokemon-card" @click="$emit('click', {name, image, types, abilities})">
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
  background-color: #2a2a2a;
  border: 2px solid #dc2626;
  padding: 1.5rem;
  margin: 0.5rem;
  border-radius: 12px;
  text-align: center;
  transition: all 0.3s ease;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: white;
}

.pokemon-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(255, 0, 0, 0.2);
  border-color: #b91c1c;
}

.pokemon-card h2 {
  color: #dc2626;
  text-transform: capitalize;
  margin-bottom: 1rem;
  font-size: 1.5rem;
}

.pokemon-image {
  width: 150px;
  height: 150px;
  object-fit: contain;
  background-color: #333;
  border-radius: 50%;
  padding: 1rem;
  margin: 1rem 0;
}

.types {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
  margin-top: 1rem;
}

.type-badge {
  padding: 0.25rem 0.75rem;
  border-radius: 16px;
  color: white;
  font-size: 0.9rem;
  text-transform: capitalize;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.abilities {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
  margin-top: 1rem;
  flex-wrap: wrap;
}

.ability-badge {
  padding: 0.25rem 0.75rem;
  border-radius: 16px;
  background-color: #333;
  color: #dc2626; 
  font-size: 0.8rem;
  text-transform: capitalize;
  border: 1px solid #dc2626;
}
</style>