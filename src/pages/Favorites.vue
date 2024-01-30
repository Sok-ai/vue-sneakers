<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://5c8e9c3f4872d278.mokky.dev/favorites')
    console.log(data)
    favorites.value = data
  } catch (error) {
    console.log(error)
  }
})
</script>


<template>
  <h1>my favorites</h1>
</template>