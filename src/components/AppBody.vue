<script setup>
import { ref } from 'vue'

const data = ref(null)
const error = ref(null)

fetch('https://fakestoreapi.com/products')
  .then((res) => res.json())
  .then((json) => (data.value = json))
  .catch((err) => (error.value = err))
</script>

<template>
  <v-container>
    <h1 class="text-h2 text-center">Супер-пупер магазин</h1>
    <v-progress-linear v-if="!data" class="mt-3" color="cyan" indeterminate></v-progress-linear>
    <v-row v-else class="d-flex justify-center mt-3">
      <ProductCard class="mt-5" v-for="product in data" :key="product.id" :product-data="product" />
    </v-row>
  </v-container>
</template>
