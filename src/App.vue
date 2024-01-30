<script setup>
import { computed, provide, ref } from 'vue'
import axios from 'axios'

import Header from './components/MyHeader.vue'
import Drawer from './components/Drawer.vue'

/* Корзина (START) */

const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, el) => acc + el.price, 0))

const vatPrice = computed(() => (totalPrice.value * 0.05).toFixed(0))

const cartIsEmpty = computed(() => cart.value.length === 0)

const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)

const drawerToReactivity = (resultBool) => (drawerOpen.value = resultBool)

const CreateOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://5c8e9c3f4872d278.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value
    })

    cart.value = []

    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreatingOrder.value = false
  }
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

/* Корзина (END) */

provide('cart', {
  cart,
  drawerToReactivity,
  addToCart,
  removeFromCart
})
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">

    <Drawer
      :total-price="totalPrice"
      :vat-price="vatPrice"
      :button-disabled="cartButtonDisabled"
      @create-order="CreateOrder"
      v-show="drawerOpen"
    />

    <Header
      :total-price="totalPrice"
      @drawer-to-reactivity="drawerToReactivity"
    />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>

</template>
 