<script setup>
defineProps({
  place: Object
})
</script>

<template>
  <div
    class="absolute inset-x-0 bottom-0 p-8 rounded-t-lg bg-white/80 backdrop-blur-sm text-slate-900"
  >
    <!-- Close button -->
    <div class="flex justify-end mb-10">
      <button @click="$emit('close-info')" class="p-1">
        <i class="text-xl fa-solid fa-xmark"></i>
      </button>
    </div>

    <div class="flex items-center justify-between gap-6 mb-20">
      <!-- Wind speed -->
      <div class="flex-1 text-center">
        <i class="text-2xl fa-solid fa-wind mb"></i>
        <p class="text-xl font-bold">{{ place.current.wind_kph }} km/h</p>
        <p>wind</p>
      </div>
      <!-- Humidity level -->
      <div class="flex-1 text-center">
        <i class="text-2xl fa-solid fa-droplet mb"></i>
        <p class="text-xl font-bold">{{ place.current.humidity }}</p>
        <p>humidity</p>
      </div>
      <!-- Precipitation -->
      <div class="flex-1 text-center">
        <i class="text-2xl fa-solid fa-umbrella mb"></i>
        <p class="text-xl font-bold">{{ place.current.precip_mm }} mm</p>
        <p>precipitation</p>
      </div>
    </div>
    <div class="flex items-center justify-between gap-6 mb-10">
      <!-- Wind direction -->
      <div class="flex-1 text-center">
        <i class="text-2xl fa-solid fa-fan mb"></i>
        <p class="text-xl font-bold">{{ place.current.wind_dir }}</p>
        <p>direction</p>
      </div>
      <!-- Feels like -->
      <div class="flex-1 text-center">
        <i class="text-2xl fa-solid fa-temperature-half mb"></i>
        <p class="text-xl font-bold">{{ Math.round(place.current.feelslike_c) }}</p>
        <p>Feels</p>
      </div>
      <!-- UV -->
      <div class="flex-1 text-center">
        <i class="text-2xl fa-solid fa-sun mb"></i>
        <p class="text-xl font-bold">{{ place.current.uv }}</p>
        <p>UV index</p>
      </div>
    </div>
    <!-- Last update and delete -->
    <div class="flex items-center justify-between">
      <h3 class="text-slate-900/50">last update: {{ place.current.last_updated }}</h3>
      <button @click="$emit('remove-place')">
        <i class="fa-solid fa-trash"></i>
      </button>
    </div>
  </div>
</template>
