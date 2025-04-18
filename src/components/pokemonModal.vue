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
          <button 
          class="add-button" 
          @click="addToPokedex"
          :disabled="isInPokedex"
        >
          {{ isInPokedex ? 'Déjà dans le Pokédex' : 'Ajouter au Pokédex' }}
        </button>
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
  },
  isInPokedex: {
    type: Boolean,
    default: false
  }
})
  
  const emit = defineEmits(['close', 'addToPokedex'])
  const addToPokedex = () => {
  if (!props.isInPokedex) {
    emit('addToPokedex', props.pokemon)
  }
}
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

  import { ref, computed, watch } from 'vue'

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
    background: #2a2a2a; 
    padding: 2rem;
    border-radius: 12px;
    max-width: 500px;
    width: 90%;
    max-height: 90vh;
    overflow-y: auto;
    position: relative;
    color: white; 
    border: 2px solid #dc2626;
  }
  
  .modal-close {
    position: absolute;
    top: 1rem;
    right: 1rem;
    border: none;
    background: none;
    font-size: 1.5rem;
    cursor: pointer;
    color: #dc2626; 
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
    background: #3a3a3a;
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
    background: #3a3a3a;
    border-radius: 16px;
    color: #dc2626; 
    border: 1px solid #dc2626;
  }

  .add-button {
  margin-top: 1rem;
  padding: 0.8rem 1.5rem;
  background-color: #dc2626;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #dc2626;
}

.add-button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
  </style>