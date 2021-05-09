<template>
  <b-card no-body class="py-2">
    <slot name="title"></slot>
    <bars
      :height="170"
      :data="datas"
      :labelData="datas"
      :labelRotate="-40"
      :rounding="2"
      :gradient="['#2ac17f', '#16975f']"
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
  background-color: #191919;
}
.card .card-body {
  padding: 10px !important;
}
</style>
