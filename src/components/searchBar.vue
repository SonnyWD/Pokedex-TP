<template>
 <div class="search-container">
    <input 
      v-model="searchQuery" 
      type="text" 
      placeholder="Rechercher un pokémon..."
      class="search-input"
    >
    <p v-if="filteredPokemons.length === 0" class="no-results">
      Aucun pokémon trouvé
    </p>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  pokemons: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['update:filtered'])
const searchQuery = ref("")

const filteredPokemons = computed(() => {
  if (!searchQuery.value.trim()) {
    return props.pokemons
  }

  const query = searchQuery.value
    .toLowerCase()
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g, "")

  return props.pokemons.filter(pokemon => {
    const name = pokemon.name
      .toLowerCase()
      .normalize("NFD")
      .replace(/[\u0300-\u036f]/g, "")
    
      const types = pokemon.types || []
    return name.includes(query) || 
           types.some(type => type.toLowerCase().includes(query))
  })
})

watch(filteredPokemons, (newValue) => {
  emit('update:filtered', newValue)
}, { immediate: true })
</script>

<style scoped>
.search-container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}

.search-input {
  width: 100%;
  padding: 0.8rem;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
}

.no-results {
  text-align: center;
  margin-top: 1rem;
  color: #666;
}
</style>