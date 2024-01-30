<script setup>
import { inject, reactive, watch, ref, onMounted } from 'vue'
import Card from '../components/Card.vue'
import axios from 'axios'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const { cart, addToCart, removeFromCart, drawerToReactivity } = inject('cart')

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const addToFavorite = async (el) => {
  try {
    if (!el.isFavorite) {
      const obj = {
        sneakerId: el.id,
        el
      }

      el.isFavorite = true

      const { data } = await axios.post('https://5c8e9c3f4872d278.mokky.dev/favorites', obj)

      el.favoriteId = data.id
    } else {
      el.isFavorite = false

      await axios.delete(`https://5c8e9c3f4872d278.mokky.dev/favorites/${el.favoriteId}`)

      el.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://5c8e9c3f4872d278.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((el) => el.sneakerId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://5c8e9c3f4872d278.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((el) => ({
    ...el,
    isAdded: cart.value.some((cartItem) => cartItem.id === el.id)
  }))
})

watch(filters, fetchItems)

watch(cart, () => {
  if (cart.value.length === 0) {
    drawerToReactivity(false)
    items.value = items.value.map((elem) => ({
      ...elem,
      isAdded: false
    }))
  }
})

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)
</script>


<template>
  <div class="flex items-center justify-between">
    <h2 class="text-3xl font-bold mb-7">Все кроссовки</h2>

    <div class="flex gap-5">
      <select
        v-model="filters.sortBy"
        class="py-2 px-3 border rounded-xl outline-none"
      >
        <option value="title">По названию</option>
        <option value="price">По цене(дешевые)</option>
        <option value="-price">По цене(дорогие)</option>
      </select>

      <div class="relative">
        <img
          class="absolute top-3 left-4"
          src="/search.svg"
          alt=""
        >
        <input
          v-model="filters.searchQuery"
          class="border rounded-xl py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Поиск..."
        >

      </div>
    </div>
  </div>

  <div class="mt-10">
    <Card
      :items="items"
      @add-to-favorite="addToFavorite"
      @add-to-cart="onClickAddPlus"
    />
  </div>
</template>