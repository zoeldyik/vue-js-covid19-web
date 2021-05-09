<template>
  <div class="data-harian mt-5">
    <h4>Data Harian</h4>

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
    <small class="text-muted">*menampilkan data 15 hari terakhir</small>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      currentPage: 1,
      perPage: 5,
      fields: [
        {
          key: "tanggal",
          stickyColumn: true,
        },
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
}

.card {
  background-color: #191919;
}
.card .table-responsive {
  margin: 0;
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
  width: auto;
  background-color: #191919;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
}

.card >>> .table tr td:first-child {
  background-color: #191919;
  color: rgba(255, 255, 255, 0.8);
  text-align: left;
}

.card >>> .pagination .page-link {
  background-color: #2ac17f;
  color: white;
  border-color: #2ac17f;
}

.card >>> .pagination .active > .page-link {
  background-color: #1eb674;
  border-color: #1eb674;
}
.card >>> small {
  padding-left: 0 !important;
}
</style>
