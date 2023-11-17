<template>
  <div class="h-20 bg-white flex">
    <div class="flex flex-col ml-5 justify-center">
      <!-- Sesuaikan nama desa masing-masing -->
      <p class="text-lg leading-none uppercase text-black font-bold mb-2">
        Indeks Pembangunan Pemuda
      </p>
    </div>
    <div class="flex items-center h-full ml-auto mr-10">
      <a class="text-lg text-black mx-4 text-end">
        <p>
          {{ namadesa }}
        </p>
        <p class="text-xm">{{ kabkot }}</p>
      </a>
    </div>
  </div>
</template>
<script>
export default {
  name: "Header",
  props: ["namadesa", "kabkot"],
};
</script>
