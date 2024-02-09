<template>
  <div class="container mx-auto p-4">
    <h1 class="text-3xl font-bold mb-4">Products</h1>
    <div v-if="loading" class="text-center">Loading...</div>
    <div v-else>
      <div v-if="error" class="text-red-500">
        Error fetching products: {{ error }}
      </div>
      <div v-else>
        <div class="mb-4">
          <product-search @search="searchProducts" />
        </div>
        <category-filter :categories="categories" @filter="filterByCategory" />
        <div
          class="
            grid grid-cols-1
            sm:grid-cols-2
            md:grid-cols-3
            lg:grid-cols-4
            gap-4
          "
        >
          <div
            v-for="(product, index) in displayedProducts"
            :key="index"
            class="bg-white shadow-md rounded-lg p-4"
          >
            <img
              :src="product.thumbnail"
              :alt="product.title"
              class="w-full h-40 object-cover mb-4"
            />
            <h2 class="text-lg font-semibold mb-2">{{ product.title }}</h2>
          </div>
        </div>
        <div class="flex justify-center mt-4">
          <button
            @click="previousPage"
            :disabled="currentPage === 1"
            class="bg-blue-500 text-white px-4 py-2 rounded mr-2"
            :class="{ 'opacity-50 cursor-not-allowed': currentPage === 1 }"
          >
            Previous
          </button>
          <button
            @click="nextPage"
            :disabled="currentPage === totalPages"
            class="bg-blue-500 text-white px-4 py-2 rounded"
            :class="{
              'opacity-50 cursor-not-allowed': currentPage === totalPages,
            }"
          >
            Next
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CategoryFilter from './CategoryFilter.vue';
import ProductSearch from './ProductSearch.vue';

export default {
  components: {
    CategoryFilter,
    ProductSearch,
  },
  data() {
    return {
      loading: false,
      products: [],
      displayedProducts: [],
      error: null,
      pageSize: 12,
      currentPage: 1,
      totalPages: 1,
      categories: [],
      selectedCategory: '',
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    fetchProducts() {
      this.loading = true;
      fetch('https://dummyjson.com/products')
        .then((response) => response.json())
        .then((data) => {
          this.products = data.products;
          this.categories = this.getCategories();
          this.updateDisplayedProducts();
          this.loading = false;
        })
        .catch((error) => {
          console.error('Error fetching products:', error);
          this.error = error.message;
          this.loading = false;
        });
    },
    updateDisplayedProducts() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = start + this.pageSize;
      this.displayedProducts = this.products.slice(start, end);
      this.totalPages = Math.ceil(this.products.length / this.pageSize);
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.updateDisplayedProducts();
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.updateDisplayedProducts();
      }
    },
    getCategories() {
      return Array.from(
        new Set(this.products.map((product) => product.category))
      );
    },
    filterByCategory(category) {
      if (category === '') {
        this.displayedProducts = this.products;
      } else {
        this.displayedProducts = this.products.filter(
          (product) => product.category === category
        );
      }
      this.selectedCategory = category;
      this.currentPage = 1;
      this.totalPages = Math.ceil(
        this.displayedProducts.length / this.pageSize
      );
    },
    searchProducts(keyword) {
      if (keyword === '') {
        this.displayedProducts = this.products;
      } else {
        this.displayedProducts = this.products.filter(
          (product) =>
            (product.title.toLowerCase().includes(keyword) ||
              product.description.toLowerCase().includes(keyword)) &&
            (product.category === this.selectedCategory ||
              this.selectedCategory === '')
        );
      }
      this.currentPage = 1;
      this.totalPages = Math.ceil(
        this.displayedProducts.length / this.pageSize
      );
    },
  },
};
</script>

<style></style>
