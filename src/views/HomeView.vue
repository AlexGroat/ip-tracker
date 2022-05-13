<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div
      class="
        flex
        justify-center
        relative
        bg-hero-pattern bg-cover
        px-4
        pt-8
        pb-32
        z-20
      "
    >
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Address Tracker</h1>
        <div class="flex">
          <input
            class="
              flex-1
              py-3
              px-2
              rounded-tl-md rounded-bl-md
              focus:outline-none
            "
            type="text"
            placeholder="Search for any IP address or leave empty to get your IP info"
          />
          <i
            class="
              fa-solid fa-chevron-right
              cursor-pointer
              bg-black
              text-white
              px-4
              rounded-tr-md rounded-br-md
              items-center
              flex
            "
          ></i>
        </div>
      </div>
      <!-- IP Info -->
      <IPInfo />
    </div>

    <!-- Map -->
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from "../components/IPInfo.vue";
import leaflet from "leaflet";
import { onMounted } from "vue";

export default {
  name: "HomeView",
  components: { IPInfo },
  setup() {
    let map;

    onMounted(() => {
      map = leaflet.map("map").setView([51.505, -0.09], 13);

      leaflet
        .tileLayer(
          "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiYWxleGcxOTk1IiwiYSI6ImNsMzNvZjJlYzAzZ3Iza3FtaGFmZTh0cWYifQ.sS6YCt3q-EJWaegnYTopFg",
          {
            attribution:
              'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: "mapbox/streets-v11",
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              "pk.eyJ1IjoiYWxleGcxOTk1IiwiYSI6ImNsMzNvZjJlYzAzZ3Iza3FtaGFmZTh0cWYifQ.sS6YCt3q-EJWaegnYTopFg",
          }
        )
        .addTo(map);
    });
  },
};
</script>
