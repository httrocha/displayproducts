<template>
  <div id="app">
    <ProductList msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import ProductList from './components/ProductList.vue';

export default {
  name: 'App',
  components: {
    ProductList,
  },
};
</script>

<style></style>
