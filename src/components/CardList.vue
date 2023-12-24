<script setup>
import Card from "./Card.vue";

defineProps({
  items: Array,
  isFavorites: Boolean,
});
const emit = defineEmits(["addToFavorite", "onClickAddPlus"]);
</script>

<template>
  <div v-auto-animate>
    <div v-if="items" class="grid grid-cols-3 gap-[10px]">
      <Card
        v-for="(item, idx) in items"
        :key="idx"
        :id="item.id"
        :title="item.title"
        :imageUrl="item.imageUrl"
        :price="item.price"
        :onClickFavorite="
          isFavorites ? null : () => emit('addToFavorite', item)
        "
        :onClickAddPlus="
          isFavorites ? null : () => emit('onClickAddPlus', item)
        "
        :isFavorite="item.isFavorite"
        :isAdded="item.isAdded"
      />
    </div>
  </div>
</template>
