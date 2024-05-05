<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import type { Ref } from 'vue'
import WindDirection from './WindDirection.vue'

type WeatherData = {
  location: {
    localtime: Date
    name: string
    region: string
  }
  current: {
    temp_c: number
    temp_f: number
    precip_mm: number
    condition: {
      text: string
      icon: string
    }
    wind_degree: number
    wind_kph: number
    wind_mph: number
  }
}

type Coords = { latitude: number; longitude: number }

interface Props {
  coords: Coords
}
const props = defineProps<Props>()

const API_BASE = 'https://api.weatherapi.com/v1'
const API_KEY = import.meta.env.VITE_APP_WEATHER_API_KEY

const data: Ref<WeatherData | undefined> = ref()

const fetchWeather = async (coords: Coords): Promise<WeatherData> => {
  const { latitude, longitude } = coords
  const q = `${latitude},${longitude}`
  const res = await fetch(`${API_BASE}/current.json?key=${API_KEY}&q=${q}`)
  const data = (await res) && res.json()

  return data
}

onMounted(async () => {
  const { latitude, longitude } = props.coords
  const weatherResponse = await fetchWeather({ latitude, longitude })
  data.value = weatherResponse
})

const formatDate = (dateString: Date): string => {
  const date = new Date(dateString)
  return new Intl.DateTimeFormat('default', {
    dateStyle: 'long',
    timeStyle: 'short'
  }).format(date)
}
</script>

<template>
  <div>
    <div class="mb-6 text-xl font-bold text-center text-red-300">
      {{
        new Date().toLocaleDateString('es-ar', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>
    <article
      v-if="data && data.current"
      class="flex max-w-md p-4 text-black bg-white rounded-lg shadow-lg w-96"
    >
      <div class="text-left basis-1/4">
        <img :src="'https:' + data.current.condition.icon" class="w-16 h-16" />
      </div>

      <div class="text-left basis-3/4">
        <h1 class="text-3xl font-bold">
          {{ data.current.condition.text }}
          <span class="block text-2xl">{{ data.current.temp_c }}°C</span>
        </h1>
        <p>{{ data.location.name }}, {{ data.location.region }}</p>
        <p>Precipitación: {{ data.current.precip_mm }}mm</p>
        <p>{{ formatDate(data.location.localtime) }}</p>
        <p>
          Viento: {{ data.current.wind_kph }} kph
          <WindDirection :degrees="data.current.wind_degree" />
        </p>
      </div>
    </article>
    <div v-else>Loading...</div>
  </div>
</template>
