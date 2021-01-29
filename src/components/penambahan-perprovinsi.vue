<template>
  <div class="data-provinsi mt-5">
    <div class="text mb-2">
      <h4>Data Harian Provinsi</h4>
      <small class="text-muted">tanggal {{ tanggal }}</small>
    </div>

    <div class="card p-1">
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

.card {
  background-color: #1c2125;
}

.card .input-group > .input-group-append > .btn {
  background-color: #9c27b0;
  margin-left: 3px;
}

.card .table-responsive {
  margin: 0;
}

.input-group > .form-control:focus {
  box-shadow: 0 0 3px 1px #9b27b0bb;
}

.card >>> .table {
  color: rgba(33, 37, 41, 0.76);
}

.card >>> .table tr th {
  width: 15%;
  text-align: right;
}

.card >>> .table tr td {
  text-align: right;
}

.card >>> .table tr th:first-child {
  background-color: #1c2125;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
  width: auto;
}

.card >>> .table tr td:first-child {
  background-color: #1c2125;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
}

.card >>> .pagination .page-link {
  background-color: #e83e8c;
  color: white;
  border-color: #e83e8c;
}

.card >>> .pagination .active > .page-link {
  background-color: #ce3b7f;
  border-color: #ce3b7f;
}
</style>
