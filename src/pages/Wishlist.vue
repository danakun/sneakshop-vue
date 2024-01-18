<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import CardGrid from '../components/CardGrid.vue'
import { inject } from 'vue'

const { onClickAdd } = inject('cart')

const favs = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://17dbc1cfc7e76f69.mokky.dev/favourites')
    favs.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <div class="text-reveal__container flex align-center justify-center">
    <h1
      class="text-reveal__span text-9xl uppercase border-b border-solid border-black text-left py-4 pl-8 font-bold"
    >
      Wishlist
    </h1>
  </div>
  <CardGrid :items="favs" @add-to-cart="onClickAdd" />
</template>

<style scoped>
.text-reveal__span {
  color: #000;
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  animation: text-reveal 5s linear;
}
</style>

<!-- 
<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import CardGrid from '../components/CardGrid.vue'
import { inject } from 'vue'

const { addToCart, removeFromCart, cart } = inject('cart')

const favs = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://17dbc1cfc7e76f69.mokky.dev/favourites')
    favs.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <div>
    <h1 class="text-9xl uppercase pl-8 border-b border-solid border-black text-left py-4 font-bold">
      Wish list
    </h1>
  </div>
  <CardGrid :items="favs" @addToCart="addToCart" @removeFromCart="removeFromCart" :cart="cart" />
</template> -->
