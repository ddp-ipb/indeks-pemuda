<script setup>
import Header from "../components/Header.vue";
</script>

<template>
  <Header />
  <main class="flex">
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

        <p>Provinsi : {{ dataipp.provinsi[0] }}</p>
        <p>
          Kabupaten/Kota :
          {{ dataipp.kabkot[0] }}
        </p>
        <p>Kecamatan : {{ dataipp.kecamatan[0] }}</p>
        <p>Desa/Kelurahan : {{ dataipp.nama_deskel[0] }}</p>
      </section>
    </div>

    <div class="flex-1 p-10">
      <h1>DOMAIN:</h1>
      <div v-if="loading">Loading...</div>
      <section v-else>
        {{ dataipp.domain }}
      </section>
      <div
        v-for="(chartData, index) in chartDataList"
        :key="index"
        class="chart-container"
      >
        <div class="chart-header">
          <h3 class="chart-title">{{ chartData.title }}</h3>
        </div>
        <div class="chart-scroll-container">
          <canvas :id="`myChart`" width="400" height="400"></canvas>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import { Chart } from "chart.js";

import axios from "axios";

export default {
  // name: "BarChart",
  // components: { Chart },
  data() {
    return {
      kode: null,
      dataipp: null,
      chartDataList: [],
      loading: true,
      errored: false,
      refreshInterval: null,
    };
  },
  methods: {
    async getIpp() {
      const response = await axios.get(
        "https://core.desapresisi.id/api/v1/indeks_pemuda?kode=" + this.kode
      );

      this.dataipp = response.data;
      console.log(response.data);

      const labels = [];
      const data = []; // Calculate the total value

      for (const key in response.data.domain) {
        if (Object.prototype.hasOwnProperty.call(response.data.domain, key)) {
          const responsescampuran = key;
          labels.push(responsescampuran);
          data.push(response.data.domain[key]);
        }
      }

      const optionsObject = {
        title: "Indeks Pembangunan Pemuda",
        labels: labels,
        data: data,
      };

      this.chartDataList = optionsObject;
      // console.log("data list", this.chartDataList);
    },

    createCharts() {
      this.chartDataList.forEach((chartData) => {
        console.log(chartData);
        const canvasId = "myChart";
        const canvasElement = document.getElementById(canvasId);

        // Check if the canvas element exists
        if (canvasElement) {
          const ctx = canvasElement.getContext("2d");
          const existingChart = Chart.getChart(ctx);

          // Destroy the existing chart if it exists
          if (existingChart) {
            existingChart.destroy();
          }

          new Chart(existingChart, {
            type: "doughnut",
            data: {
              labels: chartData.labels,
              datasets: [
                {
                  data: chartData.data,
                },
              ],
            },
            options: {
              // parsing: false,
              // normalized: true,
              // animation: false,
              // responsive: false, // Disable chart responsiveness
              // maintainAspectRatio: false, // Disable aspect ratio constraints
              // width: 50, // Set the width to 50 pixels
              // height: 200, // Set the height to 200 pixels (adjust as needed)
              // plugins: {
              //   legend: {
              //     display: true, // Hide the legend
              //   },
              //   tooltip: {
              //     callbacks: {},
              //   },
              //   onClick: function (event, elements) {},
              // },
            },
          });
        }
      });
    },
    startDataRefresh() {
      // Set up a timer to periodically refresh chart data
      // this.refreshInterval = setInterval(() => {
      //   this.loading = true;
      //   this.createCharts();
      //   this.loading = false; // Hide the progress bar when loading is complete
      // }, 1000); // Refresh data every 60 seconds (adjust as needed)
    },
    stopDataRefresh() {
      // Clear the data refresh timer
      // clearInterval(this.refreshInterval);
    },
  },
  mounted() {
    const queryParameters = this.$route.query;
    // console.log(queryParameters.kode);
    this.kode = queryParameters.kode;
    this.getIpp();
    this.createCharts();
    this.startDataRefresh();
  },
  beforeUnmount() {
    // this.stopDataRefresh(); // Stop the data refresh timer when the component is destroyed
  },
};
</script>

<style>
body {
  background: #f1f1f1;
}
</style>
