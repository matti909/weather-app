<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['place-data'])

const API_KEY = import.meta.env.VITE_APP_WEATHER_API_KEY

const API_BASE = 'http://api.weatherapi.com/v1'
const API_END1 = `${API_BASE}/search.json?key=${API_KEY}&q=`
const API_END2 = `${API_BASE}/forecast.json?key=${API_KEY}`

const searchTerm = reactive({
  query: '',
  timeout: null,
  results: null
})

const handleSearch = () => {
  clearTimeout(searchTerm.timeout)
  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query !== '') {
      const res = await fetch(API_END1 + searchTerm.query)

      const data = await res.json()
      searchTerm.results = data
    } else {
      searchTerm.results = null
    }
  }, 500)
}

const getWeather = async (id) => {
  const res = await fetch(`${API_END2}&q=id:${id}&days=3&aqi=no&alerts=no`)

  const data = await res.json()

  emit('place-data', data)

  searchTerm.query = ''
  searchTerm.results = null
}
</script>

<template>
  <div>
    <!-- search field -->
    <form>
      <div class="flex items-center bg-white border rounded-lg shadow-lg border-indigo-600/30">
        <i class="p-2 text-indigo-600 fa-solid fa-magnifying-glass"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="w-full p-2 border-0 rounded-r-lg outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset"
          v-model="searchTerm.query"
          @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="my-2 bg-white rounded-lg shadow-lg">
      <div v-if="searchTerm.results !== null">
        <div v-for="place in searchTerm.results" :key="place.id">
          <button
            @click="getWeather(place.id)"
            class="w-full px-3 my-2 text-left hover:text-indigo-600 hover:font-bold"
          >
            {{ place.name }}, {{ place.region }}, {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
