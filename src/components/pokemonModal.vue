<template>
    <div v-if="show" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <button class="modal-close" @click="closeModal">&times;</button>
        <div v-if="pokemon && Object.keys(pokemon).length > 0" class="pokemon-details">
          <h2>{{ pokemon.name }}</h2>
          <img :src="pokemon.image" :alt="pokemon.name" class="pokemon-image"/>
          <div class="types">
            <span 
              v-for="type in pokemon.types" 
              :key="type"
              class="type-badge"
              :style="{ backgroundColor: typeColors[type] }"
            >
              {{ type }}
            </span>
          </div>
          <div class="stats">
            <h3>Statistiques</h3>
            <div v-for="stat in pokemon.stats" :key="stat.name" class="stat-row">
              <span class="stat-name">{{ stat.name }}:</span>
              <span class="stat-value">{{ stat.value }}</span>
            </div>
          </div>
          <div class="abilities">
            <h3 class="ability">Capacités</h3>
            <span 
              v-for="ability in pokemon.abilities" 
              :key="ability" 
              class="ability-badge"
            >
              {{ ability }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    show: {
      type: Boolean,
      default: false
    },
    pokemon: {
      type: Object,
      default: () => ({})
    }
  })
  
  const emit = defineEmits(['close'])
  
  const closeModal = () => {
    emit('close')
  }
  
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

  import { watch } from 'vue'

watch(() => props.pokemon, (newPokemon) => {
  console.log("Données reçues par le modal :", newPokemon)
})
watch(() => props.show, (newShow) => {
  console.log("Statut du modal :", newShow ? "Ouvert" : "Fermé")
})
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .modal-content {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    max-width: 500px;
    width: 90%;
    max-height: 90vh;
    overflow-y: auto;
    position: relative;
  }
  
  .modal-close {
    position: absolute;
    top: 1rem;
    right: 1rem;
    border: none;
    background: none;
    font-size: 1.5rem;
    cursor: pointer;
  }
  
  .pokemon-details {
    text-align: center;
  }
  
  .pokemon-image {
    width: 200px;
    height: 200px;
    margin: 1rem 0;
  }
  
  .stats {
    margin: 2rem 0;

  }

  .ability {
    margin: 0 auto;
  }
  
  .stat-row {
    display: flex;
    justify-content: space-between;
    margin: 0.5rem 0;
    padding: 0.5rem;
    background: #f5f5f5;
    border-radius: 4px;
  }
  
  .type-badge {
    padding: 0.25rem 0.75rem;
    border-radius: 16px;
    color: white;
    margin: 0 0.25rem;
  }
  
  .abilities {
    margin-top: 2rem;
  }
  
  .ability-badge {
    display: inline-block;
    padding: 0.25rem 0.75rem;
    margin: 0.25rem;
    background: #f0f0f0;
    border-radius: 16px;
  }
  </style>