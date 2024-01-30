<script setup>
defineProps({
  id: Number,
  title: String,
  price: Number,
  imageUrl: String
})

const emit = defineEmits(['onClickRemove'])
</script>


<template>
  <div class="flex items-center w-full gap-5 border border-slate-200 rounded-2xl p-4 ">

    <img
      :src="imageUrl"
      :alt="title"
      class="w-20"
    >

    <div class="flex flex-col w-52">
      <p class="text-sm w-36">{{ title }}</p>

      <div class="flex justify-between items-center mt-2">
        <b>{{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          src="/close.svg"
          alt="Close"
          class="opacity-30 cursor-pointer transition hover:scale-105 hover:opacity-100"
        >
      </div>

    </div>
  </div>
</template>