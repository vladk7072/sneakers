<script setup>
const emit = defineEmits(["onClickRemove"]);
defineProps({
  item: {
    id: Number,
    title: String,
    imageUrl: String,
    price: Number,
  },
  addToCart: Function,
});
</script>

<template>
  <div
    class="border border-slate-200 px-4 py-2 rounded-xl flex justify-between items-center gap-4"
  >
    <img class="w-20 h-20" :src="item.imageUrl" :alt="item.title" />
    <div class="flex flex-col mr-auto">
      <p>{{ item.title }}</p>
      <span
        ><b>{{ item.price }} uan</b></span
      >
    </div>
    <img
      @click="emit('onClickRemove')"
      class="opacity-40 cursor-pointer transition-opacity hover:opacity-100"
      src="/close.svg"
      alt="Close"
    />
  </div>
</template>
