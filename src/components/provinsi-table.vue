<template>
  <div class="data-provinsi mt-5">
    <h4>Data Provinsi</h4>

    <div class="card p-1">
      <div class="col-md-4 offset-md-8 p-0 pt-1 mb-1">
        <b-input-group size="sm">
          <b-form-input
            v-model="filter"
            type="search"
            id="filterInput"
            placeholder="Type to Search"
          ></b-form-input>
          <b-input-group-append>
            <b-button
              class="btn-filter"
              :disabled="!filter"
              @click="filter = ''"
              >Clear</b-button
            >
          </b-input-group-append>
        </b-input-group>
      </div>

      <b-table
        borderless
        responsive
        table-variant="light"
        :per-page="perPage"
        :current-page="currentPage"
        :filter="filter"
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
  },
  data() {
    return {
      filter: null,
      filterOn: [],
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
  background-color: #9c27b0;
  color: white;
  border-color: #9c27b0;
}

.card >>> .pagination .active > .page-link {
  background-color: #7a058f;
  border-color: #7a058f;
}
</style>