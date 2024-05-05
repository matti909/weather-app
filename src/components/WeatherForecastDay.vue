<script setup>
defineProps({
  day: Object
})
</script>

<template>
  <div class="flex items-center justify-between mb-2">
    <table class="w-full">
      <tr>
        <!-- day of the week -->
        <td class="w-1/3">
          {{ new Date(day.date).toLocaleDateString('en-us', { weekend: 'long' }) }}
        </td>

        <!-- icon -->
        <td class="w-1/3">
          <img src="" alt="icon" width="30" class="mx-auto" />
        </td>

        <!-- high/low temp -->
        <td class="w-1/3 text-right">
          {{ Math.round(day.day.maxtemp_c) }} / {{ Math.round(day.day.mintemp_c) }}
        </td>
      </tr>
    </table>
  </div>
</template>
