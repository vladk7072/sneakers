<script setup>

defineProps({
  title: String,
  description: String,
  imageUrl: String
})

</script>

<template>
  <div class="max-w-full w-[300px] m-auto flex flex-col items-center justify-center">
    <img height="100" width="100" :src="imageUrl" alt="Cart image">
    <h2 class="my-3 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400 text-center">{{ description }}</p>
  </div>
</template>