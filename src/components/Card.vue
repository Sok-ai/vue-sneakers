<script setup>
import CardList from './CardList.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-10" v-auto-animate>
    <CardList
      v-for="item in items"
      :key="item"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      :isAdded="item.isAdded"
      :on-click-add="() => emit('addToCart', item)"
      :isFavorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>