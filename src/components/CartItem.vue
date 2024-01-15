<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFav: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFav: Function
})
</script>

<template>
  <div class="border border-solid border-black p-4 mx-4 flex items-center gap-4">
    <img :src="imageUrl" :alt="title" class="w-16" />
    <div class="flex justify-between w-full">
      <div>
        <p>{{ title }}</p>
        <p class="font-bold">€ 125</p>
      </div>
      <img
        @click="emit('onClickRemove')"
        src="/close.svg"
        alt="close"
        class="w-6 opacity-50 transition cursor-pointer hover:opacity-100 self-baseline"
      />
    </div>
  </div>
</template>
