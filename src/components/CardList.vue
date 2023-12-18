<script setup>
import Card from "./Card.vue";

defineProps({
  items: Array,
});

const onClickAdd = () => {
  console.log("onClickAdd");
};
</script>

<template>
  <div class="grid grid-cols-3 gap-[10px]">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
