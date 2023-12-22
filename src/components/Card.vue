<script setup>

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAddPlus: Function
});
</script>

<template>
  <div
    class="bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition-all hover:-translate-y-2 hover:shadow-xl"
  >
    <div class="relative">
      <img
        class="absolute top-0 left-0"
        :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
        alt="Like"
        @click.stop="onClickFavorite"
      />
      <img :src="imageUrl" alt="Sneakers 1" />
    </div>
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена: </span>
        <b>{{ price }} $</b>
      </div>
      <img
        @click.stop="onClickAddPlus"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>
