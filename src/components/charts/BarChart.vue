<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />

  <!-- {{ dataChart }} -->
</template>

<script>
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

const labels = [
  "pendidikan",
  "kesehatan_kesejahteraan",
  "ketenagakerjaan_kesempatan_kerja",
  "partisipasi_kepemimpinan",
  "gender_diskriminasi",
  "ipp",
];

export default {
  name: "BarChart",
  components: { Bar },
  props: ["dataChart"],

  data() {
    return {
      loading: true,
      refreshInterval: null,
      chartData: {
        labels: labels,
        datasets: [
          {
            label: this.dataChart.title,
            backgroundColor: ["Orange"],
            data: this.dataChart.data,
          },
        ],
      },
      chartOptions: {
        responsive: true,
      },
    };
  },
  methods: {
    startDataRefresh() {
      // Set up a timer to periodically refresh chart data
      this.refreshInterval = setInterval(() => {
        this.loading = true;
        // this.data();
        this.dataChart;
        this.loading = false; // Hide the progress bar when loading is complete
      }, 1000); // Refresh data every 60 seconds (adjust as needed)
    },
    stopDataRefresh() {
      // Clear the data refresh timer
      clearInterval(this.refreshInterval);
    },
  },
  mounted() {
    this.startDataRefresh();
  },
};
</script>
