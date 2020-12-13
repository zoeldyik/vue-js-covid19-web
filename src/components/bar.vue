<template>
  <b-card no-body class="py-2">
    <slot name="title"></slot>
    <bars
      :height="170"
      :data="datas"
      :labelData="datas"
      :labelRotate="-40"
      :rounding="2"
      :gradient="['#e83e8c', '#9c27b0']"
    >
    </bars>
  </b-card>
</template>

<script>
export default {
  props: {
    datas: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.card {
  background-color: #1c2125;
}
.card .card-body {
  padding: 10px !important;
}
</style>
