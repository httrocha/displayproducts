<template>
  <div>
    <input
      v-model="searchTerm"
      @input="handleSearch"
      type="text"
      placeholder="Search products..."
      class="
        w-full
        py-2
        px-4
        mb-4
        border border-gray-300
        rounded-md
        focus:outline-none focus:ring focus:border-blue-300
      "
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: '',
    };
  },
  methods: {
    handleSearch() {
      this.$emit('search', this.searchTerm.trim().toLowerCase());
    },
  },
};
</script>

<style></style>
