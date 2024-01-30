<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center flex-1 mt-56 text-center max-w-72 mx-auto">
    <img
      class="w-32"
      :src="imageUrl"
      alt="Info image"
    >
    <h2 class=" text-xl mb-3 font-semibold">{{ title }}</h2>
    <p class="text-base text-gray-400">{{ description }}</p>
  </div>
</template>