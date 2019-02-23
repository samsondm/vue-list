<template>
  <div v-if="isLoading" class="loading">Loading...</div>
  <div v-else-if="isLoadingError" class="loading-error">Error loading!</div>
  <div v-else :class="[className]">
    <slot/>
  </div>
</template>

<script>
export default {
  name: 'Loading',
  props: {
    className: String,
    isLoading: Boolean,
    isLoadingError: Boolean,
  },
};
</script>

<style lang="scss" scoped>
.loading {
  text-align: center;
  color: aqua;
}

.loading-error {
  text-align: center;
  color: red;
}
</style>


