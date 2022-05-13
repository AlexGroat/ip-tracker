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
            v-model="queryIp"
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
            @click="getIpInfo"
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
      <IPInfo v-if="ipInfo" v-bind:ipInfo="ipInfo" />
    </div>

    <!-- Map -->
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from "../components/IPInfo.vue";
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: { IPInfo },
  setup() {
    let map;

    const queryIp = ref("");
    const ipInfo = ref(null);

    onMounted(() => {
      map = leaflet.map("map").setView([-34.92, 138.6], 12);

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

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://geo.ipify.org/api/v2/country?apiKey=at_Tm53UuTujjjmaHJxbyF6jE3F8Bnk0&ipAddress=${queryIp.value}`
        );
        const res = data.data;
        console.log(res)
        ipInfo.value = {
          address: res.ip,
          state: res.location.region,
          timezone: res.location.timezone,
          isp: res.isp,
          lat: res.location.lat,
          lng: res.location.lng,
        };
        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(map);
        map.setView([ipInfo.value.lat, ipInfo.value.lng], 13);
      } catch (err) {
        alert(err.message);
      }
    };
    return { queryIp, ipInfo, getIpInfo };
  },
};
</script>
