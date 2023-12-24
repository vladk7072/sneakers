<script setup>
const props = defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAddPlus: Function,
});
</script>

<template>
  <div
    class="flex flex-col bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition-all hover:-translate-y-2 hover:shadow-xl"
  >
    <div class="relative">
      <img
        v-if="onClickFavorite"
        :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
        alt="Like"
        @click.stop="onClickFavorite"
      />
      <img class="aspect-square object-contain w-full h-full" :src="imageUrl" alt="Sneakers 1" />
    </div>
    <p class="mt-2 mb-auto">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена: </span>
        <b>{{ price }} $</b>
      </div>
      <img
        v-if="onClickFavorite"
        @click.stop="onClickAddPlus"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>
