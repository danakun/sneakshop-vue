<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
// in defineProps we put the props we want to use in the component
const props = defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFav: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFav: Function
})

//need to define here add-to-cart and like
</script>

<template>
  <div
    class="flex flex-col relative min-h-96 border border-solid border-black p-8 transition cursor-pointer hover:-translate-y-2 hover:ease-in-out hover:duration-500 hover:shadow-md"
  >
    <img
      :src="isFav ? '/like-2.svg' : '/like-1.svg'"
      alt="like"
      class="absolute top-8 left-8"
      @click="onClickFav"
    />
    <img :src="imageUrl" alt="sneakers" class="" />
    <p class="mt-5 text-lg flex-grow">{{ title }}</p>
    <div class="bottom-block flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-stone-500 uppercase">price</span>
        <span class="font-bold text-black">€ {{ price }}</span>
      </div>
      <img
        :src="isAdded ? '/checked-green.svg' : '/add-to-cart.svg'"
        alt="add to cart button"
        @click="onClickAdd"
      />
    </div>
  </div>
</template>

<!-- <template>
  <div
    class="flex flex-col relative min-h-96 border border-solid border-black p-8 transition cursor-pointer hover:-translate-y-2 hover:ease-in-out hover:duration-500 hover:shadow-md"
  >
    <img
      :src="isFav ? '/like-2.svg' : '/like-1.svg'"
      alt="like"
      class="absolute top-8 left-8"
      @click="onClickFav"
    />
    <img :src="imageUrl" alt="sneakers" class="" />
    <p class="mt-5 text-lg flex-grow">{{ title }}</p>
    <div class="bottom-block flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-stone-500 uppercase">price</span>
        <span class="font-bold text-black">€ {{ price }}</span>
      </div>
      <button @click="isAdded ? onClickRemoveFromCart() : onClickAddToCart()">
        <img :src="isAdded ? '/checked-green.svg' : '/add-to-cart.svg'" alt="add to cart button" />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    id: Number,
    imageUrl: String,
    title: String,
    price: Number,
    isFav: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFav: Function
  },
  methods: {
    onClickAddToCart() {
      this.onClickAdd(this.id)
    },
    onClickRemoveFromCart() {
      this.onClickFav(this.id)
    }
  }
}
</script> -->
