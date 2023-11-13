<script setup>
import Header from "../components/Header.vue";
</script>

<template>
  <Header />
  <main class="flex">
    <!-- <Sidebar /> -->

    <div class="flex-1 p-10">
      <section v-if="errored">
        <p>
          We're sorry, we're not able to retrieve this information at the
          moment, please try back later
        </p>
      </section>
      <div v-if="loading">Loading...</div>

      <section v-else>
        <h1>IPP : {{ dataipp.domain.ipp }}</h1>

        <p>Nama Provinsi : {{ dataipp.provinsi[0] }}</p>
        <p>
          Kabupaten/Kota :
          {{ dataipp.kabkot[0] }}
        </p>
        <p>Nama Kecamatan : {{ dataipp.kecamatan[0] }}</p>
        <p>Nama Desa/Kelurahan : {{ dataipp.nama_deskel[0] }}</p>
      </section>
    </div>

    <div class="flex-1 p-10">
      <h1>DOMAIN:</h1>
      <div v-if="loading">Loading...</div>
      <section v-else>
        {{ dataipp.domain }}
      </section>
    </div>

    <div class="flex-1 p-10">
      <h1>INDIKATOR:</h1>
      <div v-if="loading">Loading...</div>

      <section v-else>
        {{ dataipp.indikator }}
      </section>
    </div>
  </main>
</template>

<script>
import axios from "axios";
// import { Chart, registerables } from "chart.js";
// import { BarChart, useBarChart } from "vue-chart-3";

// Chart.register(...registerables);

export default {
  data() {
    return {
      kode: null,
      dataipp: null,
      loading: true,
      errored: false,
    };
  },
  methods: {
    getIpp() {
      axios
        .get("http://127.0.0.1:3000/api/v1/indeks_pemuda?kode=" + this.kode)
        .then((response) => {
          // handle success
          this.dataipp = response.data;
          console.log(response.data);
        })
        .catch(function (error) {
          // handle error
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
  mounted() {
    const queryParameters = this.$route.query;
    console.log(queryParameters.kode);
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
