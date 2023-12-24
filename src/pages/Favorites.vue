<script setup>
import { onMounted, ref } from "vue";
import CardList from "../components/CardList.vue";
import axios from "axios";

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://1d8397323be8df4a.mokky.dev/favorites?_relations=items`
    );
    favorites.value = data.map((obj) => obj.item);
  } catch (err) {
    console.log(err);
  }
});
</script>

<template>
  <h1 class="text-3xl font-bold mr-auto mb-8">Закладки:</h1>
  <CardList :items="favorites" is-favorites />
</template>
