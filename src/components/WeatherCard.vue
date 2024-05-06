<script setup>
import { ref } from 'vue'
import BorderLine from './BorderLine.vue'
import WeatherForecastDay from './WeatherForecastDay.vue'
import WeatherInfo from './WeatherInfo.vue'

defineProps({
  place: Object
})

const emit = defineEmits(['delete-place'])

const showDetail = ref(false)

const removePlace = (placeName) => {
  emit('delete-place', placeName)
  showDetail.value = false
}
</script>

<template>
  <div
    :class="place.current.is_day === 1 ? 'bg-day' : 'bg-night'"
    class="relative gap-6 p-10 mb-6 overflow-hidden text-white rounded-lg shadow-lg"
  >
    <!-- Location & time -->
    <div class="flex items-center justify-between mb-2">
      <div class="flex items-center gap-2">
        <i class="fas fa-map-marker-alt"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center gap-2">
        <i class="far fa-clock"></i>
        <h1 class="text-3xl">
          {{ new Date(place.location.localtime).getHours().toString().padStart(2, '0') }}:
          {{ new Date(place.location.localtime).getMinutes().toString().padStart(2, '0') }}
        </h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center">
      <img
        :src="place.current.condition.icon"
        alt="Weather Icon"
        width="100"
        class="mx-auto mb-4"
      />
      <h1 class="mb-2 text-4xl font-bold">{{ Math.round(place.current.temp_c) }}&deg;C</h1>
      <p class="text-xl">{{ place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div>
      <template v-for="(day, idx) in place.forecast.forecastday" :key="idx">
        <WeatherForecastDay :day="day" />
      </template>
    </div>

    <!-- info -->
    <Transition name="fade">
      <div v-show="showDetail">
        <WeatherInfo
          :place="place"
          @close-info="showDetail = flase"
          @remove-place="removePlace(place.location.name)"
        />
      </div>
    </Transition>

    <!-- forecast btn -->
    <div class="flex items-center justify-end mt-6">
      <button
        @click="showDetail = true"
        class="px-4 py-2 text-white bg-indigo-400 rounded-md hover:bg-indigo-700"
      >
        More <i class="ml-1 fas fa-arrow-right"></i>
      </button>
    </div>
  </div>
</template>

<style scoped>
.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
.bg-night {
  background-color: #07223d;
  background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
