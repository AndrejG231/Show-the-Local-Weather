<template>
  <div class="w-full h-screen flex flex-col">
    <Title />
    <div class="flex-grow bg-blue-200 flex justify-center align-center">
      <GetWeatherButton v-if="!weatherData" v-on:click="getWeather" />
      <WeatherInfo v-if="weatherData" v-bind:weatherData="weatherData" />
    </div>
  </div>
</template>

<script>
import { WeatherInfo, GetWeatherButton, Title } from "@/components/";
const components = { WeatherInfo, GetWeatherButton, Title };

export default {
  name: "app",
  components,

  data() {
    return {
      weatherData: null,
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
          .then((data) => (this.weatherData = data))
          .catch(this.locationAlert);
      }, this.locationAlert);
    },
  },
};
</script>
