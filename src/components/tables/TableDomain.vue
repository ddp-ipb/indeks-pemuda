<script setup>
import { VueTable } from "@harv46/vue-table";
import "@harv46/vue-table/dist/style.css";
</script>

<template>
  <div style="width: 100vw; 0; margin: 0 auto">
    <!-- <div> -->
    <VueTable
      :headers="header"
      :data="datatables.domain"
      :keys="keys"
      :dark="false"
    />
  </div>
</template>

<script>
const header = [
  "Pendidikan",
  "Kesehatan & Kesejahteraan",
  "Ketenagakerjaan & Kesempatan Kerja",
  "Partisipasi & Kepemimpinan",
  "Gender & Diskriminasi",
];
const keys = [
  "pendidikan",
  "kesehatan_kesejahteraan",
  "ketenagakerjaan_kesempatan_kerja",
  "partisipasi_kepemimpinan",
  "gender_diskriminasi",
];

export default {
  props: ["datatables"],
};
</script>
