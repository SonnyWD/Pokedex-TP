<template>
  <main>
    <div>
      <SearchBar 
      :pokemons="results" 
      @update:filtered="filteredResults = $event"
    />
    </div>
    <div v-if="loading">Chargement...</div>
    <div v-else class="pokemon-grid">
      <PokemonCard
        v-for="pokemon in filteredResults"
        :key="pokemon.name"
        :name="pokemon.name"
        :url="pokemon.url"
        :image="pokemon.image"
        :types="pokemon.types"
      />
    </div>
  </main>
</template>

<script setup>
import PokemonCard from './components/pokemonCard.vue'
import { ref, onMounted } from 'vue'
import SearchBar from './components/searchBar.vue'

const results = ref([])
const loading = ref(true)
const filteredResults = ref([])

const fetchPokemons = async () => {
  try {
    loading.value = true
    const response = await fetch("https://pokeapi.co/api/v2/pokemon?limit=100")
    const data = await response.json()
    
    // Créer un tableau de promesses pour récupérer les détails de chaque pokémon
    const pokemonDetails = await Promise.all(
      data.results.map(async (pokemon) => {
        const detailResponse = await fetch(pokemon.url)
        const details = await detailResponse.json()
        return {
          name: pokemon.name,
          image: details.sprites.front_default,
          types: details.types.map(type => type.type.name)
        }
      })
    )
    
    results.value = pokemonDetails
    filteredResults.value = pokemonDetails
  } catch (error) {
    console.error("Erreur:", error)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchPokemons()
})
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding: 20px;
}

.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  max-width: 1400px;
  width: 100%;
}

.loading {
  text-align: center;
  font-size: 1.5rem;
  padding: 2rem;
}
</style>