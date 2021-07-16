<template>
  <div class="w-full h-screen flex flex-col">
    <Title />
    <div class="flex-grow bg-blue-200 flex justify-center align-center">
      <GetWeatherButton v-if="!weahterData" v-on:click="getWeather" />
      <WeatherInfo v-if="weahterData" />
    </div>
  </div>
</template>

<script>
import * as components from "@/components/";

export default {
  name: "app",
  components,
  data() {
    return {
      weahterData: null,
    };
  },
  methods: {
    locationAlert() {
      alert("Could not retrieve location from your browser!");
    },
    // Get weather data
    getWeather() {
      navigator.geolocation.getCurrentPosition((success) => {
        const { latitude, longitude } = success.coords;
        //

        if (typeof latitude !== "number" && typeof longitude !== "number") {
          return this.locationAlert();
        }

        fetch(
          `https://weather-proxy.freecodecamp.rocks/api/current?lat=${latitude}&lon=${longitude}`
        )
          .then((res) => res.json())
          .then((data) => (this.weahterData = data))
          .catch(this.locationAlert);
      }, this.locationAlert);
    },
  },
};
</script>
