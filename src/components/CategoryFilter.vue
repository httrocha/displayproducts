<template>
  <div>
    <label for="category-select" class="block text-sm font-medium text-gray-700"
      >Filter by Category:</label
    >
    <select
      id="category-select"
      v-model="selectedCategory"
      @change="filterByCategory"
      class="
        mt-1
        block
        w-full
        py-2
        px-3
        border border-gray-300
        bg-white
        rounded-md
        shadow-sm
        focus:outline-none focus:ring-indigo-500 focus:border-indigo-500
        sm:text-sm
      "
    >
      <option value="">All</option>
      <option v-for="category in categories" :key="category">
        {{ category }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    categories: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selectedCategory: '',
    };
  },
  methods: {
    filterByCategory() {
      this.$emit('filter', this.selectedCategory);
    },
  },
};
</script>

<style></style>
