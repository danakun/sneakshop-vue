<script setup>
import { inject } from 'vue'
defineProps({
  title: String,
  description: String,
  imageUrl: String
})

const { closeDrawer } = inject('cart')
</script>

<template>
  <div class="flex flex-col items-center gap-4 mt-2 w-72 mx-auto text-center">
    <img :src="imageUrl" alt="image" class="w-100 self-center" />
    <h2 class="mt-4 text-3xl font-bold">{{ title }}</h2>
    <p class="text-stone-500">{{ description }}</p>
    <button
      @click="closeDrawer"
      class="w-full rounded bg-black transition py-3 text-white p-2 my-4 hv-color hover:ease-in-out hover:duration-500"
    >
      Return to home
    </button>
  </div>
</template>
