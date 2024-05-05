<script setup>
import BorderLine from './BorderLine.vue'
import WeatherForecastDay from './WeatherForecastDay.vue'

defineProps({
  place: Object
})
</script>

<template>
  <div class="relative gap-6 p-10 mb-6 overflow-hidden text-white rounded-lg shadow-lg">
    <!-- Location & time -->
    <div class="flex items-center justify-between mb-2">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">
          {{ new Date(place.location.localtime).getHours() }}:{{
            new Date(place.location.localtime).getMinutes()
          }}
        </h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="flex-1 text-center">
      <img :src="place.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="mb-2 -mr-4 text-9xl">{{ Math.round(place.current.temp_c) }}&deg;</h1>
      <p class="text-2xl">{{ place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="(day, idx) in place.forecast.forecastday" :key="idx">
      <WeatherForecastDay :day="day" />
    </div>

    <!-- info -->
    <div>
      <!-- Weather info component goes here -->
    </div>

    <!-- forecast btn -->
    <div class="flex items-center justify-end gap-1 mt-10">
      <button>More <i class="-mb-px text-sm fa-solid fa-arrow-right"></i></button>
    </div>
  </div>
</template>
