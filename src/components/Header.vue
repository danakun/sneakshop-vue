<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { defineEmits } from 'vue'
defineProps({
  totalPrice: Number
})
const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-solid border-black px-8">
    <router-link to="/"
      ><div class="pr-8">
        <img src="/logo-sneakshop.svg" alt="sneakshop logo" class="w-100 m-auto" />
      </div>
    </router-link>

    <ul class="flex items-baseline justify-evenly text-stone-500 gap-10 pt-8">
      <li>
        <a
          @click="emit('openDrawer')"
          class="flex flex-row gap-3 font-bold transition hover:invert hover:brightness-150 hover:ease-in-out hover:duration-500"
          href="#"
          ><img src="/cart.svg" alt="cart icon" class="w-6 price" />€{{ totalPrice }}</a
        >
      </li>
      <router-link to="/wishlist"
        ><li>
          <a
            class="flex flex-row gap-3 transition hover:invert hover:brightness-150 hover:ease-in-out hover:duration-500"
            href="#"
            ><img src="/heart.svg" alt="heart icon" class="w-6" />Wishlist</a
          >
        </li>
      </router-link>
      <li>
        <a
          class="flex flex-row gap-3 transition hover:invert hover:brightness-150 hover:ease-in-out hover:duration-500"
          href="#"
          ><img src="/profile.svg" alt="profile icon" class="w-6" />Profile</a
        >
      </li>
    </ul>
  </header>
</template>
