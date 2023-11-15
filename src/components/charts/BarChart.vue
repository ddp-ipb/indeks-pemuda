<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />
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

export default {
  name: "BarChart",
  components: { Bar },
  data() {
    return {
      chartData: {
        labels: [
          "Lama Studi",
          "APK-Sekolah Menengah",
          "APK-Perguruan Tinggi",
          "Bebas Penyakit",
          "Keikutsertakan Jamkes",
          "Pemuda Kerah Putih",
          "Pengangguran Terbuka",
          "Partisipasi Organisasi",
          "APK-SMA-Perempuan",
          "Pemudi Pekerja",
        ],
        datasets: [{ data: [40, 20, 12, 23, 23, 23, 23, 24, 56, 78, 78, 79] }],
      },
      chartOptions: {
        responsive: true,
      },
    };
  },
};
</script>
