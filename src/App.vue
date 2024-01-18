<script setup>
import { computed, onMounted, onUnmounted } from 'vue'
import { ref } from 'vue'

import { watch, provide } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'
import Footer from './components/Footer.vue'

// reactive states for items and filters, ref for primitives and arrays, reactive for objects

// onClickAdd is a function that adds or removes an item from the cart
// addToCart is a function that pushes an item to the cart
const onClickAdd = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

/* Cart Logic */
const cart = ref([])

//here we declare Drawer as a ref
const drawerOpened = ref(false)
// we create a function to close the drawer
const closeDrawer = () => {
  drawerOpened.value = false
}
const openDrawer = () => {
  drawerOpened.value = true
}
// I want to make a func to close the drawer on esc button

const closeOnEsc = () => {
  const handleEscKey = (e) => {
    if (e.key === 'Escape') {
      closeDrawer()
    }
  }

  onMounted(() => {
    window.addEventListener('keydown', handleEscKey)
  })

  onUnmounted(() => {
    window.removeEventListener('keydown', handleEscKey)
  })
}

closeOnEsc()
// here we create a func to calculate the total price
// it is a callback function that gets executed for each element in the cart.value array during the reduction process. It takes two parameters:
// acc: This is an accumulator that keeps track of the running total.
// item: This represents each item in the cart.value array as we iterate through it.
// acc + item.price: In each iteration, the callback function adds the price property of the current item to the accumulator acc. This is how the total price accumulates as we go through each item in the cart.
//though the result is not a reactive value!!! it will be calculated on mounted ant thats it, so it gives 0
// const totalPrice = cart.value.reduce((acc, item) => acc + item.price, 0)
//so we need the special method - computed
// computed helps to watch for changes in the totalPrice
const totalPrice = computed(() => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
})

const vatPrice = computed(() => {
  return Math.round(totalPrice.value * 0.21)
})

// const cartButtonDisabled = computed(() => cart.value.length === 0 || isOrdering.value)

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

// we watch for changes in cart in localStorage using deep watch,
// because we want to watch for changes in cart array
watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)
// we globally provide the cart actions
provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
  onClickAdd
})
/* Cart Logic End */
</script>

<template>
  <Drawer v-if="drawerOpened" :total-price="totalPrice" :vat-price="vatPrice" />
  <div class="bg-white w-4/5 m-auto rounded-md border border-solid border-black shadow-md my-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <!-- route outlet -->
    <!-- component matched by the route will render here -->
    <router-view></router-view>
    <Footer />
  </div>
</template>

<style scoped></style>
