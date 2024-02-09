<template>
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
      :class="{ 'opacity-50 cursor-not-allowed': currentPage === totalPages }"
    >
      Next
    </button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true,
    },
    totalPages: {
      type: Number,
      required: true,
    },
  },
  methods: {
    nextPage() {
      this.$emit('next');
    },
    previousPage() {
      this.$emit('prev');
    },
  },
};
</script>

<style></style>
