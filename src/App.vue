<script setup>
import { ref } from 'vue'
import GetLocation from './components/GetLocation.vue'
import SearchFile from './components/SearchFile.vue'
import WeatherCard from './components/WeatherCard.vue'

const places = ref([])
console.log(places)

const addPlace = (data) => {
  places.value.push(data)
}
</script>

<template>
  <div class="container">
    <GetLocation />

    <div>
      <SearchFile @place-data="addPlace" />
    </div>

    <!--Weather cards-->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place, idx) in places" :key="idx">
        <WeatherCard :place="place" />
      </div>
    </div>
  </div>
</template>
