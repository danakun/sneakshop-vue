<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import DrawerHeader from './DrawerHeader.vue'
import CartList from './CartList.vue'
import InfoBlock from './InfoBlock.vue'
import axios from 'axios'
import { ref } from 'vue'
import { inject, computed } from 'vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { cart } = inject('cart')

const isOrdering = ref(false)
const orderId = ref(null)

// const emit = defineEmits(['makeOrder'])
// const makeOrder = () => {
//   emit('makeOrder')
// }

const makeOrder = async () => {
  try {
    isOrdering.value = true
    const { data } = await axios.post('https://17dbc1cfc7e76f69.mokky.dev/orders', {
      items: cart.value,
      total: props.totalPrice.value
    })
    cart.value = []

    orderId.value = data.id
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isOrdering.value = false
    // if I delete item before the order is made, it must be unchecked from the home page
    // items.value = items.value.map((item) => {
    //   if (item.isAdded) {
    //     return { ...item, isAdded: false }
    //   }
    //   return item
    // })
  }
}
const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(() => isOrdering.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black bg-opacity-70 z-20"></div>
  <div
    class="bg-white w-96 h-full fixed top-0 right-0 border border-solid border-black py-2 z-30 flex flex-col flex-1"
  >
    <DrawerHeader />
    <div v-if="!totalPrice || orderId" class="flex h-full items-center justify-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        class="empty-cart"
        title="Nothing here"
        description="Add at least one item to the cart"
        imageUrl="/skull.svg"
      />

      <InfoBlock
        v-if="orderId"
        class="order-placed"
        :title="`Your order #${orderId} is placed successfully!`"
        description="You will receive an order confirmation email shortly."
        imageUrl="/boot.svg"
      />
    </div>

    <CartList />
    <div v-if="totalPrice" class="px-4">
      <div class="flex justify-between gap-2 mb-2 my-4">
        <span class="">TOTAL:</span>
        <div class="flex-1 border-b border-solid border-black"></div>
        <span class="font-bold">€{{ totalPrice }}</span>
      </div>

      <div class="flex justify-between gap-2 w-full my-2">
        <span class="">TAX 21%:</span>
        <div class="flex-1 border-b border-solid border-black"></div>
        <span class="font-bold">€{{ vatPrice }}</span>
      </div>

      <button
        :disabled="cartButtonDisabled"
        @click="makeOrder"
        class="w-full rounded bg-black transition py-3 text-white p-2 my-4 hv-color hover:ease-in-out hover:duration-500"
      >
        Checkout
      </button>
    </div>
  </div>
</template>

<style>
.hv-color:hover {
  background-color: var(--green-color);
}
.hv-color:active {
  background-color: lawngreen;
}

.hv-color:disabled {
  background-color: grey;
  cursor: not-allowed;
}
</style>
