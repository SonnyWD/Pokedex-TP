<template>
  <main>
    <div class="controls">
      <SearchBar 
        :pokemons="results" 
        @update:filtered="filteredResults = $event"
      />
      <SortByName
        :pokemons="filteredResults"
        @update:sorted="filteredResults = $event"
      />
      <sortByTypes
      :pokemons="filteredResults"
      @update:sorted="filteredResults = $event"
      /> 
    </div>
    <div class="load-more">
        <button 
          @click="loadMore" 
          class="load-more-button"
          :disabled="loading"
        >
          Charger plus de Pokémon
        </button>
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
        :abilities="pokemon.abilities"
      />
    </div>
  </main>
</template>

<script setup>
import PokemonCard from './components/pokemonCard.vue'
import { ref, onMounted } from 'vue'
import SearchBar from './components/searchBar.vue'
import SortByName from './components/sortByName.vue'
import sortByTypes from './components/sortByTypes.vue'

const results = ref([])
const loading = ref(true)
const filteredResults = ref([])
const offset = ref(0)
const limit = 100
const loadMoreLimit = 5

const fetchPokemons = async () => {
  try {
    loading.value = true
    const currentLimit = offset.value === 0 ? limit : loadMoreLimit
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=${currentLimit}&offset=${offset.value}`)
    const data = await response.json()
    
    const newPokemonDetails = await Promise.all(
      data.results.map(async (pokemon) => {
        const detailResponse = await fetch(pokemon.url)
        const details = await detailResponse.json()
        return {
          name: pokemon.name,
          image: details.sprites.front_default,
          types: details.types.map(type => type.type.name),
          abilities: details.abilities.map(ability => ability.ability.name)
        }
      })
    )
    
    results.value = [...results.value, ...newPokemonDetails]
    filteredResults.value = [...results.value]
    offset.value += limit
  } catch (error) {
    console.error("Erreur:", error)
  } finally {
    loading.value = false
  }
}
const loadMore = () => {
  fetchPokemons()
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
.controls {
  display: flex;
  gap: 1rem;
  align-items: center;
  margin-bottom: 2rem;
}

.load-more {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
}

.load-more-button {
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.load-more-button:hover {
  background-color: #357abd;
}

.load-more-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>