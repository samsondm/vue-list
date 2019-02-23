<template>
  <div>
    <div>{{details.description}}</div>
    <div v-if="details.links.website[0]">
      <a
        :href="details.links.website[0]"
        :title="details.name"
        target="_blank"
        rel="noopener noreferrer"
      >Home page</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CryptoListDetails',
  props: {
    details: {
      description: String,
      links: Object,
    },
  },
};
</script>

