<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { inject, ref, reactive, watch, onMounted } from 'vue'

import axios from 'axios'
import Hero from '../components/Hero.vue'
import CardGrid from '../components/CardGrid.vue'

const { addToCart, removeFromCart, cart } = inject('cart')

const items = ref([])
// we store sortBy and searchQuery in the filter object
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

// onClickAdd is a function that adds or removes an item from the cart
// addToCart is a function that pushes an item to the cart
const onClickAdd = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}
// we create functions for onChangeSelect and onChangeSearchInput,
//they watch for changes in sortBy and searchQuery and pass them to the fetchItems function

// const sortBy = ref('')
// const searchQuery = ref('')
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

// function to add item to favourites or remove it
const addToFavs = async (item) => {
  try {
    if (!item.isFav) {
      const obj = {
        parentId: item.id,
        item
      }

      // for better ux we inmediately set isFav to true
      item.isFav = true
      const { data } = await axios.post('https://17dbc1cfc7e76f69.mokky.dev/favourites', obj)
      // console.log(data)
      // we save id in item.favId to know which id has the fav you want to remove
      item.favId = data.id
    } else {
      item.isFav = false
      // here we delete the fav using its favId and turn it false
      await axios.delete(`https://17dbc1cfc7e76f69.mokky.dev/favourites/${item.favId}`)
      item.favId = null
    }
  } catch (err) {
    console.log(err)
  }

  console.log(item)
}

// func for fetching favourites, we receive data from backend, save it in the favs array

const fetchFavs = async () => {
  try {
    const { data: favourites } = await axios.get('https://17dbc1cfc7e76f69.mokky.dev/favourites')
    // we map over the favourites array and look for favs, saying that parentId is equal to item.id
    // we compare array of items and array of favourites, and if id matches we put isFav in items array
    items.value = items.value.map((item) => {
      const fav = favourites.find((fav) => fav.parentId === item.id)

      if (!fav) {
        return item
      }
      return {
        ...item,
        isFav: true,
        favId: fav.id
      }
    })

    console.log(items.value)
    console.log(favourites)
  } catch (err) {
    console.log(err)
  }
}

// we create a function which fetches the data from backend at the first render or further changes
const fetchItems = async () => {
  // we sort and check if there is a search query
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    // here we use axios to get the data from mokky.dev and pass the data to the CardGrid
    const { data } = await axios.get('https://17dbc1cfc7e76f69.mokky.dev/items', { params })
    // we use map to create a new array with data about likes and isAdded
    items.value = data.map((obj) => ({
      ...obj,
      isFav: false,
      favId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(
  // fetch('https://17dbc1cfc7e76f69.mokky.dev/items').then((res) =>
  //   res.json().then((data) => console.log(data))
  // )

  //   axios.get('https://17dbc1cfc7e76f69.mokky.dev/items').then((res) => console.log(res.data))
  //
  async () => {
    // we get data from local storage if the cart is not empty and we parse it to an array
    const localCart = localStorage.getItem('cart')
    cart.value = localCart ? JSON.parse(localCart) : []

    await fetchItems()
    await fetchFavs()

    items.value = items.value.map((item) => {
      return {
        ...item,
        isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
      }
    })
  }
)

// thanks to watch we can watch for changes in cart array and than we renew the items
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

//a method to watch for changes
watch(
  // () => filters.sortBy + filters.searchQuery, if we want to watch both
  filters,
  fetchItems
)
</script>

<template>
  <Hero />
  <div class="">
    <h2 class="text-9xl font-bold mt-10 px-8 pb-8 border-b border-solid border-black">
      ALL SNEAKERS
    </h2>
    <div class="flex justify-between px-8 pt-6">
      <!-- <div>
          <span class="text-3xl font-bold">FILTERS</span>
        </div>
        -->
      <div class="flex gap-4">
        <span class="text-3xl font-bold">SORT BY</span>
        <select
          @change="onChangeSelect"
          class="py-1.5 px-4 cursor-pointer hover:shadow-[0_0_0_2px_rgba(0,0,0,1)] hover:outline-none hover:rounded-3xl focus:outline-none active:shadow-[0_0_0_2px_rgba(0,0,0,1)]"
        >
          <option value="name">Model</option>
          <!-- <option value="*Nike*">Only Nike</option> -->
          <option value="price">Cheap First</option>
          <option value="-price">Expensive First</option>
        </select>
      </div>

      <div class="relative">
        <img src="/search2.svg" alt="search" class="absolute left-4 top-2.5" />
        <input
          name="search"
          @input="onChangeSearchInput"
          placeholder="Search..."
          type="text"
          class="py-1.5 pr-4 pl-10 rounded-3xl border border-solid border-black outline-none focus:box-contain focus:border-black focus:shadow-[0_0_0_2px_rgba(0,0,0,1)]"
        />
      </div>
    </div>
    <CardGrid :items="items" @add-to-favs="addToFavs" @add-to-cart="onClickAdd" />
  </div>
</template>
