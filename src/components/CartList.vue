<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

// we do destructuring here, not to receive an object with cart inside
const { cart, removeFromCart } = inject('cart')
</script>

<template>
  <div class="flex flex-col flex-1 gap-4 py-4 overflow-y-auto custom-scrollbar" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>

<style scoped>
.custom-scrollbar::-webkit-scrollbar {
  width: 12px;
  height: 14px;
  background-color: transparent;
}

.custom-scrollbar {
  /* this gives us natural scrolling behaviour on touch screens */
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: var(--green-color);
  border: solid 2px #fff;
}

.schedule {
  padding: 48px 24px;
}
</style>
