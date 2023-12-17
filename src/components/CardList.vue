<script setup>
import Card from "./Card.vue";

const onClickAdd = () => {
  console.log("onClickAdd");
};

const onClickFavorite = () => {
  console.log("onClickFavorite");
};

</script>

<template>
  <div class="grid grid-cols-4 gap-[10px]">
    <Card
      title="1Мужские Кроссовеи Nike Blazer Mid Suede"
      imageUrl="/sneakers/sneakers-1.jpg"
      :price="3599"
      :is-added="false"
      :is-favorite="false"
      :onClickAdd="onClickAdd"
      :onClickFavorite="onClickFavorite"
    />
  </div>
</template>
