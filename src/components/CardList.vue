<script setup>
import Card from "./Card.vue";

defineProps({
  items: Array,
});
const emit = defineEmits(["addToFavorite", "onClickAddPlus"]);
</script>

<template>
  <div class="grid grid-cols-3 gap-[10px]">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :onClickAddPlus="() => emit('onClickAddPlus', item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
