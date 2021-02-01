<template>
  <div class="list-item">
    <h3>
      {{ source.title }}
    </h3>
    <div v-html="source.snippet"></div>
  </div>
</template>

<script>
export default {
  props: {
    index: {
      // index of current item
      type: Number,
    },
    source: {
      type: Object,
      default() {
        return {};
      },
    },
  },
};
</script>

<style scoped>
.list-item {
  padding: 0 10px 20px 10px;
}
.list-item h3 {
  margin: 0;
  padding-bottom: 10px;
}
</style>
