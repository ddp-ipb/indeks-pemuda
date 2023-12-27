<template>
  <div class="h-20 bg-white flex">
    <div class="flex flex-col justify-center">
      <!-- Sesuaikan nama desa masing-masing -->

      <div class="flex items-center p-10 h-full">
        <a href="https://ipp.kemenpora.go.id/">
          <img
            src="@/assets/ipp-logo.png"
            height="20"
            width="150"
            alt="Index Pembangunan Pemuda"
          />
        </a>
        <a
          class="text-lg text-gray-500 ml-20 mx-4 cursor-pointer active:text-black hover:text-black"
          href="https://ipp.kemenpora.go.id/"
          >Indeks
          <!-- <router-link to=""></router-link> -->
        </a>

        <a
          class="text-lg text-gray-500 mx-4 cursor-pointer hover:text-black"
          href="https://ipp.kemenpora.go.id/about"
          >Tentang
          <!-- <router-link to=""></router-link> -->
        </a>

        <a
          class="text-lg text-gray-500 mx-4 cursor-pointer hover:text-black"
          href="https://ipp.kemenpora.go.id/faq"
          >Tanya Jawab
          <!-- <router-link to=""></router-link> -->
        </a>

        <a
          class="text-lg text-gray-500 mx-4 cursor-pointer hover:text-black"
          href="https://ipp.kemenpora.go.id/document"
          >Dokumen
          <!-- <router-link to=""></router-link> -->
        </a>
      </div>

      <!-- <p class="text-lg leading-none uppercase text-black font-bold mb-2">
        Indeks Pembangunan Pemuda
      </p> -->
    </div>
    <div class="flex items-center h-full ml-auto">
      <a class="text-xm text-gray-500 text-end hover:text-black cursor-pointer">
        <p>
          {{ namadesa }}
        </p>
        <!-- <p class="text-xm">{{ date }}</p> -->
      </a>
    </div>

    <div class="flex items-center h-full">
      <a
        class="text-xm text-gray-500 mr-10 mx-4 bg-[#0ea5e9] py-1 px-2 rounded text-end hover:text-black cursor-pointer"
      >
        <p class="text-xm text-white">{{ date }}</p>
      </a>
    </div>
  </div>
</template>
<script>
export default {
  name: "Header",
  props: ["namadesa", "kabkot", "date"],
};
</script>
