<script setup>
import Header from "../components/layout/Header.vue";
import Footer from "../components/layout/Footer.vue";
import Map from "../components/Map.vue";
import Table from "../components/tables/Table.vue";
import TableDomain from "../components/tables/TableDomain.vue";
import BarChart from "../components/charts/BarChart.vue";
// import Doughnut from "../components/charts/Doughnut.vue";
</script>

<template>
  <Header
    v-bind:namadesa="`DESA ` + namadesa"
    v-bind:kabkot="`KAB. ` + kabkot"
    v-bind:date="date"
  ></Header>
  <main>
    <div class="flex flex-row mr-5">
      <div class="flex-1 p-5 ml-5 md:flex-col md:max-w-xl">
        <a
          href="#"
          class="flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row md:max-w-xl hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700"
        >
          <img
            class="mx-10 h-20 p-2 rounded-lg"
            src="@/assets/high-score.png"
            alt="image description"
          />
          <div class="flex flex-col justify-between p-4 leading-normal">
            <h5
              class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white"
            >
              Skor IPP
            </h5>
            <div class="mb-3 font-normal text-gray-700 dark:text-gray-400">
              <div v-if="loading">Loading...</div>
              <section v-else>
                <h1 class="font-bold text-xl">
                  {{ dataipp.domain[0].ipp }}
                  <span class="font-normal text-xs">/100</span>
                </h1>
              </section>
            </div>
          </div>
        </a>
      </div>

      <div class="flex-1 p-5 ml-5">
        <a
          href="#"
          class="flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row md:max-w-xl hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700"
        >
          <img
            class="mx-10 h-20 p-2 rounded-lg"
            src="@/assets/group.png"
            alt="image description"
          />
          <div class="flex flex-col justify-between p-4 leading-normal">
            <h5
              class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white"
            >
              Jiwa
            </h5>
            <div class="mb-3 font-normal text-gray-700 dark:text-gray-400">
              <div v-if="loading">Loading...</div>
              <section v-else>
                <h1 class="font-bold text-xl">
                  {{ dataipp.jiwa }}
                </h1>
              </section>
            </div>
          </div>
        </a>
      </div>

      <div class="flex-1 p-5 ml-5">
        <a
          href="#"
          class="flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row md:max-w-xl hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700"
        >
          <img
            class="mx-10 h-20 p-2 rounded-lg"
            src="@/assets/pemuda.png"
            alt="image description"
          />
          <div class="flex flex-col justify-between p-4 leading-normal">
            <h5
              class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white"
            >
              Pemuda
            </h5>
            <div class="mb-3 font-normal text-gray-700 dark:text-gray-400">
              <div v-if="loading">Loading...</div>
              <section v-else>
                <h1 class="font-bold text-xl">
                  {{ dataipp.pemuda }}
                </h1>
              </section>
            </div>
          </div>
        </a>
      </div>
    </div>

    <!-- Start indikator -->
    <div class="max-w-none overflow-hidden px-6 ml-5">
      <h2>Indikator</h2>
    </div>

    <!-- <div class="mb-2 table-fixed">
      <div class="flex-1">
        <div class="max-w-fit overflow-hidden">
          <div class="px-6 py-2 table-auto">
            <div class="font-normal text-xm">
              <Table v-bind:datatables="datatables"></Table>
            </div>
          </div>
        </div>
      </div>
    </div> -->

    <div class="relative overflow-x-auto mx-10 m-5">
      <div
        class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
      >
        <Table v-bind:datatables="datatables"></Table>
      </div>
    </div>

    <!-- Start Domain -->
    <div class="max-w-none overflow-hidden mx-10">
      <h2>Domain</h2>
    </div>

    <div class="relative overflow-x-auto mx-10 m-5">
      <div
        class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
      >
        <TableDomain v-bind:datatables="datatables"></TableDomain>
      </div>
    </div>

    <!-- <div class="flex mb-2">
      <div class="flex-1 p-5">
        <div class="max-w-none rounded bg-white overflow-hidden">
          <div class="px-6 py-2">
            <BarChart v-bind:dataChart="chartDataList"></BarChart>
          </div>
        </div>
      </div>
    </div> -->

    <div class="max-w-none rounded overflow-hidden bg-white shadow-lg mx-10">
      <h2 class="p-2">Peta Sebaran Pemuda</h2>
      <div class="">
        <Map v-bind:datamarkers="markers"></Map>
        <!-- <Map v-bind:datamarkers="markers"></Map> -->
      </div>
    </div>
  </main>
  <Footer />
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  components: { Map, Table, BarChart },
  data() {
    return {
      kode: null,
      dataipp: null,
      namadesa: "-",
      kabkot: "-",
      date: "-",
      datamarkers: [],
      chartDataList: [],
      loading: true,
      errored: false,
      datatables: [],
    };
  },
  methods: {
    getIpp() {
      axios
        .get(
          "https://core.desapresisi.id/api/v1/indeks_pemuda?kode=" + this.kode
        )
        .then((response) => {
          // handle success
          this.dataipp = response.data;
          this.namadesa = response.data.nama_deskel[0].toUpperCase();
          this.kabkot = response.data.kabkot[0].toUpperCase();
          const newdate = new Date(response.data.tanggal[0]);
          this.date = newdate.getFullYear();
          this.datamarkers = response.data.sebaran;
          this.datatables = response.data;

          console.log(newdate.getFullYear());

          // this.chartDataList = response.data.domain;

          const labels = [];
          const data = []; // Calculate the total value

          for (const key in response.data.domain[0]) {
            {
              const responsescampuran = key;
              // console.log(key[0]);
              labels.push(responsescampuran);
              data.push(response.data.domain[0][key]);
            }
          }

          const optionsObject = {
            title: "Domain IPP",
            labels: labels,
            data: data,
          };

          this.chartDataList = optionsObject;
          // console.log(optionsObject);
        })
        .catch(function (error) {
          // handle error
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },

  computed: {
    markers() {
      return this.datamarkers
        .filter((br) => br.latitude && br.longitude)
        .map((br) => ({
          ...br,
          markerLatlng: [parseFloat(br.latitude), parseFloat(br.longitude)],
        }));
    },
  },
  mounted() {
    const queryParameters = this.$route.query;
    this.kode = queryParameters.kode;
    this.getIpp();
  },
};
</script>

<style>
body {
  background: #f1f1f1;
}
</style>
