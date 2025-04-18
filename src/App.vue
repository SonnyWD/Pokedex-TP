<template>
  <main>
    <h1 class="title">Pokédex</h1>
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
        @click="() => openModal(pokemon)" 
      />
    </div>
    <PokemonModal 
      :show="showModal"
      :pokemon="selectedPokemon"
      :isInPokedex="isPokemonInPokedex(selectedPokemon)"
      @close="closeModal"
      @add-to-pokedex="handleAddToPokedex"
    />
  </main>
</template>

<script setup>
import PokemonCard from './components/pokemonCard.vue'
import { ref, onMounted } from 'vue'
import SearchBar from './components/searchBar.vue'
import SortByName from './components/sortByName.vue'
import sortByTypes from './components/sortByTypes.vue'
import PokemonModal from './components/pokemonModal.vue'

const results = ref([])
const loading = ref(true)
const filteredResults = ref([])
const offset = ref(0)
const limit = 100
const loadMoreLimit = 5
const showModal = ref(false)
const selectedPokemon = ref(null)
const myPokedex = ref([])

const openModal = (pokemon) => {
  selectedPokemon.value = pokemon
  showModal.value = true
}

const isPokemonInPokedex = (pokemon) => {
  if (!pokemon) return false
  return myPokedex.value.some(p => p.name === pokemon.name)
}

const handleAddToPokedex = (pokemon) => {
  if (!isPokemonInPokedex(pokemon)) {
    myPokedex.value.push(pokemon)
    localStorage.setItem('myPokedex', JSON.stringify(myPokedex.value))
  }
}

const closeModal = () => {
  showModal.value = false
  selectedPokemon.value = null
}

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
          abilities: details.abilities.map(ability => ability.ability.name),
          stats: details.stats.map(stat => ({
            name: stat.stat.name,
            value: stat.base_stat
          }))
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
  const savedPokedex = localStorage.getItem('myPokedex')
  if (savedPokedex) {
    myPokedex.value = JSON.parse(savedPokedex)
  }
  fetchPokemons()
})
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding: 20px;
  background-color: #1a1a1a;
  min-height: 100vh;
}

.title {
  text-align: center;
  font-size: 3rem;
  color: #dc2626; 
  margin-bottom: 2rem;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 2px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
}

.controls {
  display: flex;
  gap: 1rem;
  align-items: center;
  margin-bottom: 2rem;
  background-color: #2a2a2a;
  padding: 1rem;
  border-radius: 10px;
}

.load-more {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
}

.load-more-button {
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  background-color: #dc2626;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.load-more-button:hover {
  background-color: #b91c1c;
}

.load-more-button:disabled {
  background-color: #666;
  cursor: not-allowed;
}
</style>