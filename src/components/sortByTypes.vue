<template>
    <div class="sort-container-types">
      <select 
        v-model="selectedType" 
        @change="updateSortTypes" 
        class="sort-select"
      >
        <option value="">Tous les types</option>
        <option v-for="type in availableTypes" :key="type" :value="type">
          {{ type }}
        </option>
      </select>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  
  const props = defineProps({
    pokemons: {
      type: Array,
      required: true
    }
  })
  
  const emit = defineEmits(['update:sorted'])
  const selectedType = ref('')
  
  // Récupère tous les types uniques des pokémons
  const availableTypes = computed(() => {
    const types = new Set()
    props.pokemons.forEach(pokemon => {
      pokemon.types.forEach(type => types.add(type))
    })
    return Array.from(types).sort()
  })
  
  const updateSortTypes = () => {
    if (!selectedType.value) {
      // Si aucun type n'est sélectionné, retourner tous les pokémons
      emit('update:sorted', props.pokemons)
      return
    }
  
    // Filtrer les pokémons qui ont le type sélectionné
    const filteredPokemons = props.pokemons.filter(pokemon => 
      pokemon.types.includes(selectedType.value)
    )
    
    emit('update:sorted', filteredPokemons)
  }
  </script>
  
  <style scoped>
  .sort-container-types {
    margin: 1rem 0;
  }
  
  .sort-select {
    padding: 0.5rem;
    border-radius: 4px;
    border: 1px solid #ccc;
    font-size: 1rem;
    background-color: white;
    cursor: pointer;
    min-width: 150px;
  }
  
  .sort-select:focus {
    outline: none;
    border-color: #4a90e2;
  }
  </style>