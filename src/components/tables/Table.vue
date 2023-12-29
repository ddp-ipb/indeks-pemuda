<script setup>
import { VueTable } from "@harv46/vue-table";
import "@harv46/vue-table/dist/style.css";
</script>

<template>
  <div style="width: 100vw; 0; margin: 0 auto">
    <!-- <div> -->
    <VueTable
      :headers="header"
      :data="datatables.indikator"
      :keys="keys"
      :dark="false"
    />
  </div>
</template>

<script>
const header = [
  "Lama Studi",
  "APK-Sekolah Menengah",
  "APK-Perguruan Tinggi",
  "Bebas Penyakit",
  "Keikutsertaan Jamkes",
  "Pemuda Kerah Putih",
  "Pengangguran Terbuka",
  "Partisipasi Organisasi",
  "Partisipasi Ormas",
  "APK-SMA Perempuan",
  "Pemudi Pekerja",
];
const keys = [
  "lama_studi",
  "apk_sm",
  "apk_pt",
  "bebas_penyakit",
  "keikutsertaan_jamkes",
  "pemuda_kerah_putih",
  "p_terbuka",
  "partisipasi_org",
  "partisipasi_orm",
  "apk_pt_pemudi",
  "pemudi_pekerja",
];

export default {
  props: ["datatables"],
};
</script>
