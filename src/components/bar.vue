<template>
  <div class="box py-2">
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
  </div>
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
.box {
  background-color: #191919;
}
.box .box-body {
  padding: 10px !important;
}
</style>
