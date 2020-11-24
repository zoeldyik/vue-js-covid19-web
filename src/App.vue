<template>
  <div id="app">
    <b-container>
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
              *menampilkan grafik berdasarkan data 10 hari terakhir
            </small>
          </b-row>
        </div>
      </div>
      <!-- end row -->

      <footer class="mt-3" v-if="dataTotal.length">
        <div class="container text-center pt-5 pb-3">
          <small>
            data berasal dari api yang di kembangkan oleh
            <a href="https://github.com/mathdroid/indonesia-covid-19-api"
              >mathdroid</a
            >
          </small>
        </div>
      </footer>
    </b-container>
  </div>
</template>

<script>
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
    };
  },
  methods: {
    getTotal() {
      fetch("https://indonesia-covid-19.mathdro.id/api")
        .then((res) => res.json())
        .then((res) => {
          // let {meninggal,sembuh,perawatan,jumlahKasus}= res;
          let data = Object.entries(res);
          data.splice(4);
          data.forEach((e) => {
            if (e[0] === "jumlahKasus") {
              this.dataTotal.push({ name: "kasus positif", value: e[1] });
            } else {
              this.dataTotal.push({ name: e[0], value: e[1] });
            }
          });

          this.dataTotal = this.dataTotal.sort((a, b) => b.value - a.value);
          // console.log(this.dataTotal);
        })
        .catch((err) => console.log(err));
    },
    getProvinsi() {
      fetch("https://indonesia-covid-19.mathdro.id/api/provinsi")
        .then((res) => res.json())
        .then((res) => {
          let data = res.data.filter((e) => e.provinsi !== "Indonesia");
          data.forEach(
            (e) => (e.dirawat = e.kasusPosi - e.kasusSemb - e.kasusMeni)
          );

          data.forEach((e) => {
            this.dataProvinsi.push({
              provinsi: e.provinsi,
              positif: e.kasusPosi,
              sembuh: e.kasusSemb,
              meninggal: e.kasusMeni,
              dirawat: e.dirawat,
            });
          });

          // console.log(data);
        })
        .catch((err) => console.log(err));
    },
    getHarian() {
      fetch("https://indonesia-covid-19.mathdro.id/api/harian")
        .then((res) => res.json())
        .then((res) => {
          let data = res.data.slice(-15);
          data = data
            .filter((e) => e.jumlahKasusKumulatif)
            .map((e) => {
              return {
                tanggal:
                  new Date(e.tanggal).getDate() +
                  "-" +
                  parseInt(new Date(e.tanggal).getMonth() + 1) +
                  "-" +
                  new Date(e.tanggal).getFullYear(),
                // spesimenHarian: e.kasusDiperiksaBaruHarian,
                positif: e.jumlahKasusBaruperHari,
                sembuh: e.jumlahKasusSembuhperHari,
                meninggal: e.jumlahKasusMeninggalperHari,
                dirawat: e.jumlahKasusDirawatperHari,
              };
            });

          this.dataHarian = data.reverse();
          this.positifHarian = data
            .map((e) => e.positif)
            .slice(0, 10)
            .reverse();
          this.sembuhHarian = data
            .map((e) => e.sembuh)
            .slice(0, 10)
            .reverse();
          // console.log(this.dataHarian);
        })
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    this.getTotal();
    this.getProvinsi();
    this.getHarian();
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
</style>
