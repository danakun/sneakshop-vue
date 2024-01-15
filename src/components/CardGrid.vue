<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
// emit is like props for funcs in parent
// here we describe all the events we want to use
const emit = defineEmits(['addToFavs', 'addToCart'])

// const onClickFav = () => {
//   emit('addToFavs', item)
// }

// here we inject the addToFavs function from the parent component
// const addToFavs = inject('addToFavs')

// const onClickFav = () => {
//   const obj = {
//     ...props,
//     parentId: props.id
//   }
//   addToFavs(obj)
// }
</script>

<template>
  <div class="grid grid-cols-4 gap-6 py-6 px-6" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isFav="item.isFav"
      :isAdded="item.isAdded"
      :onClickAdd="() => emit('addToCart', item)"
      :onClickFav="() => emit('addToFavs', item)"
    />
  </div>
</template>
