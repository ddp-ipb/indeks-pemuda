<script setup>
import Header from "../components/Header.vue";
</script>

<template>
  <Header v-bind:namadesa="`DESA ` + namadesa"></Header>
  <main>
    <div class="flex">
      <div class="flex-1 p-5">
        <div class="max-w-xs rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-5">
            <div class="text-m mb-2">Indeks Pembangunan Pemuda</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-bold text-xl mb-2">{{ dataipp.domain.ipp }}</h1>
            </section>
          </div>
        </div>
      </div>

      <div class="flex-1 p-5">
        <div class="max-w-xs rounded overflow-hidden bg-white shadow-lg">
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
        <div class="max-w-xs rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-5">
            <div class="text-m mb-2">Jumlah Pemuda</div>
            <div v-if="loading">Loading...</div>
            <section v-else>
              <h1 class="font-bold text-xl mb-2">{{ dataipp.pemuda }}</h1>
            </section>
          </div>
        </div>
      </div>

      <!-- <div v-if="loading">Loading...</div>

        <section v-else>
          <h1>IPP : {{ dataipp.domain.ipp }}</h1>

          <p>Provinsi : {{ dataipp.provinsi[0] }}</p>
          <p>
            Kabupaten/Kota :
            {{ dataipp.kabkot[0] }}
          </p>
          <p>Kecamatan : {{ dataipp.kecamatan[0] }}</p>
          <p>Desa/Kelurahan : {{ dataipp.nama_deskel[0] }}</p>
        </section> -->
      <!-- </div> -->
    </div>

    <div class="flex">
      <!-- <div class="flex-1 p-5">
        <div class="max-w-xs rounded overflow-hidden bg-white shadow-lg">
          <div class="px-6 py-5">
            <div class="text-m mb-2">Indikator Pembangunan Pemuda</div>
            <div v-if="loading">Loading...</div>
            <BarChart />
          </div>
        </div>
      </div> -->

      <!-- <div class="flex-1 p-10">
        <div>
          <BarChart />
        </div>
      </div>

      <div class="flex-1 p-10">
        <div>
          <BarChart />
        </div>
      </div> -->
    </div>

    <div
      class="max-w-none rounded overflow-hidden bg-white shadow-lg ml-5 mr-5"
    >
      <div class="px-6 py-10">
        <Map />
      </div>
    </div>
  </main>
</template>

<script>
import BarChart from "../components/charts/BarChart.vue";
import axios from "axios";
import Map from "../components/Map.vue";
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

const post = {
  id: 1,
  title: "My Journey with Vue",
};

export default {
  name: "BarChart",
  components: { BarChart, Map },
  data() {
    return {
      kode: null,
      dataipp: null,
      namadesa: "Nama Desa",
      chartDataList: [],
      loading: true,
      errored: false,
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
          // console.log(response.data);

          const labels = [];
          const data = [];

          for (const key in response.data.domain) {
            if (
              Object.prototype.hasOwnProperty.call(response.data.domain, key)
            ) {
              const responsescampuran = key;
              labels.push(responsescampuran);
              data.push(response.data.domain[key]);
            }
          }

          const optionsObject = {
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
    createCharts() {
      this.chartDataList.forEach((chartData) => {
        console.log(chartData);
        const canvasId = "myChart";
        const canvasElement = document.getElementById(canvasId);

        // Check if the canvas element exists
        if (canvasElement) {
          const ctx = canvasElement.getContext("2d");
          const existingChart = ChartJS.getChart(ctx);

          // Destroy the existing chart if it exists
          if (existingChart) {
            existingChart.destroy();
          }

          new Chart(ctx, {
            type: "BarChart",
            data: {
              labels: [
                "January",
                "February",
                "March",
                "April",
                "May",
                "June",
                "July",
                "August",
                "September",
                "October",
                "November",
                "December",
              ],
              datasets: [
                {
                  data: [40, 20, 12, 39, 10, 40, 39, 80, 40, 20, 12, 11],
                },
              ],
            },
            options: {
              parsing: false,
              normalized: true,
              animation: false,
              responsive: true, // Disable chart responsiveness
              maintainAspectRatio: false, // Disable aspect ratio constraints
              width: 50, // Set the width to 50 pixels
              height: 200, // Set the height to 200 pixels (adjust as needed)
              plugins: {
                legend: {
                  display: true, // Hide the legend
                },
                tooltip: {
                  callbacks: {},
                },
                onClick: function (event, elements) {},
              },
            },
          });
        }
      });
    },
  },
  mounted() {
    const queryParameters = this.$route.query;
    // console.log(queryParameters.kode);
    this.kode = queryParameters.kode;
    this.getIpp();
    // this.createCharts();
  },
};
</script>

<style>
body {
  background: #f1f1f1;
}
</style>
