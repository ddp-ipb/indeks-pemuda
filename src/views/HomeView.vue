<script setup>
import Header from "../components/layout/Header.vue";
import Footer from "../components/layout/Footer.vue";
import Map from "../components/Map.vue";
import Table from "../components/tables/Table.vue";
import BarChart from "../components/charts/BarChart.vue";
</script>

<template>
  <Header
    v-bind:namadesa="`DESA ` + namadesa"
    v-bind:kabkot="`KAB. ` + kabkot"
  ></Header>
  <main>
    <div class="flex">
      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-5">
            <div class="text- mb-2">Indeks Pembangunan Pemuda</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-bold text-xl mb-2">
                {{ dataipp.domain[0].ipp }}
              </h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-5">
            <div class="text-m mb-2">Jumlah Jiwa</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-bold text-xl mb-2">{{ dataipp.jiwa }}</h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-5">
            <div class="text-m mb-2">Jumlah Pemuda</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-bold text-xl mb-2">{{ dataipp.pemuda }}</h1>
            </section>
          </div>
        </div>
      </div>
    </div>

    <!-- Start indikator -->
    <div class="max-w-none overflow-hidden px-6">
      <h2>Indikator</h2>
    </div>

    <div class="mb-2">
      <div class="flex-1 mr-5">
        <div class="max-w-full overflow-hidden">
          <div class="px-6 py-2">
            <div class="font-normal text-xm">
              <Table v-bind:datatables="datatables"></Table>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Start Domain -->
    <div class="max-w-none overflow-hidden px-6 py-0">
      <h2>Domain</h2>
    </div>

    <div class="flex">
      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-2">
            <div class="text-xs mb-2">Pendidikan</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-light text-xm mb-2">
                {{ dataipp.domain[0].pendidikan }}
              </h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-2">
            <div class="text-xs mb-2">Kesehatan & Kesejahteraan</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-light text-xm mb-2">
                {{ dataipp.domain[0].kesehatan_kesejahteraan }}
              </h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-2">
            <div class="text-xs mb-2">Ketenagakerjaan & Kesempatan Kerja</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-light text-xm mb-2">
                {{ dataipp.domain[0].ketenagakerjaan_kesempatan_kerja }}
              </h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-2">
            <div class="text-xs mb-2">Partisipasi & Kepemimpinan</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-light text-xm mb-2">
                {{ dataipp.domain[0].partisipasi_kepemimpinan }}
              </h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-auto rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-2">
            <div class="text-xs mb-2">Gender & Diskriminasi</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-light text-xm mb-2">
                {{ dataipp.domain[0].gender_diskriminasi }}
              </h1>
            </section>
          </div>
        </div>
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

    <div
      class="max-w-none rounded overflow-hidden bg-white shadow-lg ml-5 mr-5"
    >
      <h2 class="p-2">Peta Sebaran</h2>
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
          this.datamarkers = response.data.sebaran;
          this.datatables = response.data;

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
          console.log(optionsObject);
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
