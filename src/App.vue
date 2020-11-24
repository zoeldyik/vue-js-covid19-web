<template>
  <div id="app">
    <!-- content -->
    <b-container v-show="sembuhHarian">
      <div class="row">
        <div class="col-md-10 offset-md-1">
          <app-header :datas="dataTotal" v-if="dataTotal.length"></app-header>
          <provinsi-table
            :items="dataProvinsi"
            v-if="dataProvinsi.length"
          ></provinsi-table>

          <harian-table
            :items="dataHarian"
            v-if="dataHarian.length"
          ></harian-table>
          <b-row class="mt-5">
            <b-col sm="6" class="mb-3 mb-md-0">
              <vue-bar v-if="positifHarian" :datas="positifHarian">
                <template v-slot:title>
                  <h6 class="text-center mb-3 mt-2">Kasus Positif Harian</h6>
                </template>
              </vue-bar>
            </b-col>
            <b-col sm="6">
              <vue-bar v-if="sembuhHarian" :datas="sembuhHarian">
                <template v-slot:title>
                  <h6 class="text-center mb-3 mt-2">Kasus sembuh Harian</h6>
                </template>
              </vue-bar>
            </b-col>

            <small
              class="text-muted"
              style="padding-left: 15px"
              v-if="positifHarian"
            >
              *menampilkan grafik berdasarkan data 14 hari terakhir
            </small>
          </b-row>
        </div>
      </div>
      <!-- end row -->

      <footer class="mt-3" v-if="dataTotal.length">
        <div class="container text-center pt-5 pb-3">
          <small>
            data berasal dari api yang di sediakan oleh pemerintah
            <a href="https://data.covid19.go.id/public/api/update.json">
              indonesia
            </a>
          </small>
        </div>
      </footer>
    </b-container>

    <!-- loader -->
    <div class="loader" v-show="showLoader">
      <div class="lds-hourglass"></div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");
import header from "./components/header.vue";
import dataProvinsi from "./components/provinsi-table.vue";
import dataHarian from "./components/harian-table.vue";
import bar from "./components/bar.vue";

export default {
  name: "App",
  components: {
    "app-header": header,
    "provinsi-table": dataProvinsi,
    "harian-table": dataHarian,
    "vue-bar": bar,
  },
  data() {
    return {
      dataTotal: [],
      dataProvinsi: [],
      dataHarian: [],
      positifHarian: null,
      sembuhHarian: null,
      showLoader: true,
    };
  },
  methods: {
    getTotal() {
      axios
        .get(
          `https://api.allorigins.win/raw?url=${encodeURIComponent(
            "https://data.covid19.go.id/public/api/update.json"
          )}`
        )
        .then((res) => {
          // console.log(res);
          // console.log(res.data.update.total);

          const {
            jumlah_positif,
            jumlah_dirawat,
            jumlah_meninggal,
            jumlah_sembuh,
          } = res.data.update.total;

          const data = [
            { name: "POSITIF", value: jumlah_positif },
            { name: "SEMBUH", value: jumlah_sembuh },
            { name: "DIRAWAT", value: jumlah_dirawat },
            { name: "MENINGGAL", value: jumlah_meninggal },
          ];

          this.dataTotal = data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getProvinsi() {
      axios
        .get(
          `https://api.allorigins.win/raw?url=${encodeURIComponent(
            "https://data.covid19.go.id/public/api/prov.json"
          )}`
        )
        .then((res) => {
          // console.log(res);
          // console.log(res.data.list_data);
          const datas = res.data.list_data;
          const temp = [];

          datas.forEach((el) => {
            temp.push({
              provinsi: el.key,
              positif: el.jumlah_kasus,
              sembuh: el.jumlah_sembuh,
              meninggal: el.jumlah_meninggal,
              dirawat: el.jumlah_dirawat,
            });
          });

          this.dataProvinsi = temp;
          // console.log(temp);
        })
        .catch((err) => console.log(err));
    },
    getHarian() {
      axios
        .get(
          `https://api.allorigins.win/raw?url=${encodeURIComponent(
            "https://data.covid19.go.id/public/api/update.json"
          )}`
        )
        .then((res) => {
          console.log(res);
          const datas = res.data.update.harian.slice(-15);
          const temp = [];
          console.log(datas);

          datas.forEach((el) => {
            temp.unshift({
              tanggal:
                new Date(el.key_as_string).getDate() +
                "-" +
                parseInt(new Date(el.key_as_string).getMonth() + 1) +
                "-" +
                new Date(el.key_as_string).getFullYear(),
              positif: el.jumlah_positif.value,
              sembuh: el.jumlah_sembuh.value,
              meninggal: el.jumlah_meninggal.value,
              dirawat: el.jumlah_dirawat.value,
            });
          });

          console.log(temp);
          this.dataHarian = temp;

          // data untuk bar
          this.positifHarian = datas
            .map((e) => e.jumlah_positif.value)
            .slice(-14);

          this.sembuhHarian = datas
            .map((e) => e.jumlah_sembuh.value)
            .slice(-14);
        })
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    this.getTotal();
    this.getProvinsi();
    this.getHarian();
    setTimeout(() => {
      this.showLoader = false;
    }, 800);
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,400;1,300&display=swap");
#app {
  font-family: "Roboto", sans-serif;
  background-color: #171b1f;
  /* background-color: #212529; */
  color: rgba(255, 255, 255, 0.9);
  min-height: 100vh;
}

.loader {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #171b1f;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

h6 {
  opacity: 0.7;
}

footer small {
  color: rgba(255, 255, 255, 0.65);
}

footer a {
  color: #7a058f;
}

footer a:hover {
  color: #9c27b0;
}

/* style for loader animation */
.lds-hourglass {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-hourglass:after {
  content: " ";
  display: block;
  border-radius: 50%;
  width: 0;
  height: 0;
  margin: 8px;
  box-sizing: border-box;
  border: 32px solid #fff;
  border-color: #fff transparent #fff transparent;
  animation: lds-hourglass 1.2s infinite;
}
@keyframes lds-hourglass {
  0% {
    transform: rotate(0);
    animation-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19);
  }
  50% {
    transform: rotate(900deg);
    animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
  }
  100% {
    transform: rotate(1800deg);
  }
}
</style>
