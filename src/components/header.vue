<template>
  <div class="row pt-4">
    <div class="col-12 mb-5 mt-4">
      <h1 class="text-center d-block">
        Data
        <span
          style="color: #e83e8c; font-weight: 400 opacity:1;"
          class="font-weight-bold"
          >Covid-19</span
        >
        Indonesia
      </h1>
    </div>

    <div class="col-md-6 mb-3" v-for="(data, index) in datas" :key="index">
      <div class="card">
        <div class="card-body d-flex">
          <div class="text">
            <h5 class="font-italic">{{ data.name }}</h5>
            <h3>{{ data.value }}</h3>
          </div>
          <b-img fluid :src="icons[index]" class="ml-auto"></b-img>
        </div>
      </div>
    </div>
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
    return {
      icons: ["virus.svg", "recover.svg", "sick.svg", "death.svg"],
    };
  },
};
</script>

<style scoped>
.card {
  background-color: #1c2125;
}

.card img {
  width: 4rem;
}

h1 {
  opacity: 0.9;
}

h5 {
  opacity: 0.8;
}

h3 {
  opacity: 0.9;
}
</style>
