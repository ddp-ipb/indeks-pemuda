<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />

  {{ dataChart }}
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
];

export default {
  name: "BarChart",
  components: { Bar },
  props: ["dataChart"],

  data() {
    return {
      chartData: {
        labels: this.labels,
        datasets: [{ data: this.dataChart.data }],
      },
      chartOptions: {
        responsive: true,
      },
    };
  },
};
</script>
