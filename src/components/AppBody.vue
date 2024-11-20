<script setup>
import { computed, ref, watch } from 'vue'

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

const minPrice = ref(null)
const maxPrice = ref(null)

watch(filteredData, (newData, prevData) => {
  const prices = newData.map((item) => item.price)
  minPrice.value = prices.length > 0 ? Math.min(...prices) : 0
  maxPrice.value = prices.length > 0 ? Math.max(...prices) : 100
})
</script>

<template>
  <v-container fluid>
    <AppHeader v-model="searchStr" />
    <v-progress-linear v-if="!data" class="mt-3" color="cyan" indeterminate></v-progress-linear>
    <v-row v-else class="mt-3">
      <v-col cols="2">
        <ProductCardSearch v-model="searchStr" :minPrice="minPrice" :maxPrice="maxPrice" />
      </v-col>
      <v-col>
        <v-row class="d-flex justify-center ">
          <ProductCard class="mt-5" v-for="product in filteredData" :key="product.id" :product-data="product" />
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>
