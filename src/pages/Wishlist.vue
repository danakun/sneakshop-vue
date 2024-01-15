<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import CardGrid from '../components/CardGrid.vue'

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
    <h1 class="text-9xl uppercase border-b border-solid border-black text-left py-4 font-bold">
      Wishlist
    </h1>
  </div>
  <CardGrid :items="favs" />
</template>
