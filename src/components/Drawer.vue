<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean
})

const emit = defineEmits(['CreateOrder'])
</script>


<template>
  <div>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70 "></div>

    <div class="h-full w-96 bg-white fixed right-0 top-0 z-20 p-8 flex flex-col overflow-auto">

      <DrawerHead />

      <InfoBlock
        v-if="!totalPrice"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />

      <template v-else>
        
        <CartItemList/>

        <div
          v-show="totalPrice"
          class="flex flex-col gap-5 mt-7"
        >

          <div class="flex gap-2">
            <span>Итого:</span>
            <div class="border-b border-dashed border-slate-300 flex-1"></div>
            <b>{{ totalPrice }} руб.</b>
          </div>

          <div class="flex gap-2">
            <span>Налог 5%:</span>
            <div class="border-b border-dashed border-slate-300 flex-1"></div>
            <b>{{ vatPrice }} руб.</b>
          </div>
          <button
            @click="emit('CreateOrder')"
            :disabled="buttonDisabled"
            class="mt-6 transition bg-lime-500 w-full py-4 text-center text-white rounded-2xl cursor-pointer disabled:cursor-default disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700"
          >
            Оформить заказ
          </button>
        </div>
      </template>
    </div>
  </div>
</template>