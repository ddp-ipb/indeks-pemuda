<script setup>
import {
  LMap,
  LControlLayers,
  LTileLayer,
  LMarker,
  LGeoJson,
} from "@vue-leaflet/vue-leaflet";
import { latLng, featureGroup } from "leaflet";
</script>

<template>
  <div class="">
    <l-map
      :zoom="zoom"
      :center="center"
      :bounds="bounds"
      :max-bounds="maxBounds"
      style="height: 500px; width: 100%"
    >
      <l-control-layers position="topright"></l-control-layers>
      <l-tile-layer
        v-for="tileProvider in tileProviders"
        :key="tileProvider.name"
        :name="tileProvider.name"
        :visible="tileProvider.visible"
        :url="tileProvider.url"
        :attribution="tileProvider.attribution"
        layer-type="base"
      />

      <l-marker
        v-for="(marker, index) in datamarkers"
        :key="index"
        :lat-lng="marker.markerLatlng"
        :name="`Pemuda`"
        :options="options"
        :options-style="styleFunction"
      >
      </l-marker>

      <l-geo-json
        :name="`Batas Desa`"
        v-if="show"
        :geojson="geojson"
        :options="options"
        layer-type="overlay"
        :options-style="styleFunction"
      />
    </l-map>
    <!-- {{ datamarkers }} -->
  </div>
</template>

<script>
const tileProviders = [
  {
    name: "Esri Imageri",
    visible: true,
    url: "https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}",
    attribution:
      "Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community",
  },
  {
    name: "OpenStreetMap",
    visible: false,
    attribution:
      '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
    url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  },
];

export default {
  name: "Leaflet",
  components: {
    LMap,
    LControlLayers,
    LTileLayer,
    LGeoJson,
    LMarker,
  },
  props: ["datamarkers"],
  data() {
    return {
      loading: true,
      show: true,
      enableTooltip: true,
      zoom: 4,
      center: [-6.175648391650097, 106.82718498185969],
      bounds: null,
      maxBounds: null,
      geojson: null,
      fillColor: "#0CF9E0",
      tileProviders: tileProviders,
    };
  },
  methods: {
    async getBatasDesa() {
      this.loading = true;
      const response = await fetch(
        "https://maps.desapresisi.id/api/geodeskel?kode=" + this.kode
      );
      const data = await response.json();
      this.geojson = data;
      this.loading = false;
    },
  },

  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction,
      };
    },
    styleFunction() {
      const fillColor = this.fillColor;
      return () => {
        return {
          weight: 2,
          color: "#000000",
          opacity: 1,
          fillColor: fillColor,
          fillOpacity: 0.5,
        };
      };
    },
    onEachFeatureFunction() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindTooltip(
          "<div>Dusun: " + feature.properties.dusun + "</div>",
          {
            permanent: false,
            sticky: true,
          }
        );
      };
    },
  },
  mounted() {
    const queryParameters = this.$route.query;
    this.kode = queryParameters.kode;
    this.getBatasDesa();
    // this.getSebaran()
  },
};
</script>
