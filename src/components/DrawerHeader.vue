<script setup>
import { inject } from 'vue'

const { closeDrawer } = inject('cart')
</script>

<template>
  <a
    @click="closeDrawer"
    href="#"
    class="flex justify-end pb-2 px-8 border-b border-solid border-black"
    ><span class="material-symbols-outlined uppercase px-3 text-2xl hover:opacity-80">close</span
    ><img
      src="/cross.svg"
      class="transition hover:transform hover:scale-125 hover:ease-in-out hover:duration-300"
  /></a>
  <h2 class="text-9xl uppercase border-b border-solid border-black text-center py-4 font-bold">
    cart
  </h2>
</template>
