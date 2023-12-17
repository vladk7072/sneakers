<script setup>
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
</script>

<template>
  <Drawer />
  <main class="w-4/5 mx-auto bg-white rounded-xl shadow-xl my-14">
    <Header />
    <div class="p-10">
      <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
      <CardList />
    </div>
  </main>
</template>
