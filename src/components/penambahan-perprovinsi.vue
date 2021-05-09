<template>
  <div class="data-provinsi mt-5">
    <div class="text mb-2">
      <h4>Data Harian Provinsi</h4>
      <small class="text-muted">tanggal {{ tanggal }}</small>
    </div>

    <div class="box p-1">
      <b-table
        borderless
        responsive
        table-variant="light"
        :per-page="perPage"
        :current-page="currentPage"
        :fields="fields"
        :items="items"
      ></b-table>

      <b-pagination
        hide-goto-end-buttons
        v-model="currentPage"
        :total-rows="totalRows"
        :per-page="perPage"
        align="right"
        size="sm"
        class="my-1"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    tanggal: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      filter: null,
      currentPage: 1,
      perPage: 7,
      fields: [
        { key: "provinsi", stickyColumn: true },
        { key: "positif" },
        { key: "sembuh" },
        { key: "meninggal" },
        { key: "dirawat" },
      ],
    };
  },
  computed: {
    totalRows() {
      return this.items.length;
    },
  },
};
</script>
<style scoped>
h4 {
  opacity: 0.8;
  margin-bottom: -5px;
}

.box {
  background-color: #191919;
}

.box .input-group > .input-group-append > .btn {
  background-color: #9c27b0;
  margin-left: 3px;
}

.box .table-responsive {
  margin: 0;
}

.input-group > .form-control:focus {
  box-shadow: 0 0 3px 1px #9b27b0bb;
}

.box >>> .table {
  color: rgba(33, 37, 41, 0.76);
}

.box >>> .table tr th {
  width: 15%;
  text-align: right;
}

.box >>> .table tr td {
  text-align: right;
}

.box >>> .table tr th:first-child {
  background-color: #191919;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
  width: auto;
}

.box >>> .table tr td:first-child {
  background-color: #191919;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
}

.box >>> .pagination .page-link {
  background-color: #2ac17f;
  color: white;
  border-color: #2ac17f;
}

.box >>> .pagination .active > .page-link {
  background-color: #1eb674;
  border-color: #1eb674;
}
</style>
