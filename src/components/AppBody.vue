<script setup>
import { computed, ref } from 'vue'

const data = ref(null)
const error = ref(null)

fetch('https://fakestoreapi.com/products')
  .then((res) => res.json())
  .then((json) => (data.value = json))
  .catch((err) => (error.value = err))

const searchStr = ref(null)

const filteredData = computed(() => {
  if (searchStr.value === null) return data.value
  else {
    return data.value.filter(product => product.title.indexOf(searchStr.value) > -1)
  }
})
</script>

<template>
  <v-container>
    <AppHeader v-model="searchStr" />
    <v-progress-linear v-if="!data" class="mt-3" color="cyan" indeterminate></v-progress-linear>
    <v-row v-else class="d-flex justify-center mt-3">
      <ProductCard class="mt-5" v-for="product in filteredData" :key="product.id" :product-data="product" />
    </v-row>
  </v-container>
</template>
