<template>
    <div class="sort-container">
      <select 
        v-model="sortOrder" 
        @change="updateSort" 
        class="sort-select"
      >
        <option value="">Sans tri</option>
        <option value="asc">A-Z</option>
        <option value="desc">Z-A</option>
      </select>
    </div>
  </template>
  
  <script setup>
  import { ref, watch } from 'vue'
  
  const props = defineProps({
    pokemons: {
      type: Array,
      required: true
    }
  })
  
  const emit = defineEmits(['update:sorted'])
  const sortOrder = ref('')
  
  const sortPokemons = (pokemons, order) => {
    if (!order) return [...pokemons]
    
    return [...pokemons].sort((a, b) => {
      if (order === 'asc') {
        return a.name.localeCompare(b.name)
      } else {
        return b.name.localeCompare(a.name)
      }
    })
  }
  
  const updateSort = () => {
    const sortedPokemons = sortPokemons(props.pokemons, sortOrder.value)
    emit('update:sorted', sortedPokemons)
  }
  
  watch(sortOrder, updateSort)
  </script>
  
  <style scoped>
  .sort-container {
    margin: 1rem 0;
  }
  
  .sort-select {
    padding: 0.5rem;
    border-radius: 4px;
    border: 1px solid #ccc;
    font-size: 1rem;
    background-color: white;
    cursor: pointer;
  }
  
  .sort-select:focus {
    outline: none;
    border-color: #4a90e2;
  }
  </style>