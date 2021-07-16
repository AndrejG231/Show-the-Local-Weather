<template>
  <div class="w-full bg-blue-300 grid grid-cols-1 md:grid-cols-2">
    <!-- Info about location -->
    <div
      class="border-b-4 md:border-b-0 md:border-r-4 border-white mx-4 md:mx-0 md:my-4 flex flex-col justify-between align-center"
    >
      <div>
        <h2 class="text-3xl md:text-7xl ml-3 text-white">
          {{ country }}
        </h2>
        <div class="border-b-2 border-b-white mr-8 mt-2 "></div>
        <h3 class="md:text-4xl text-2xl ml-6 text-white">
          {{ state }}
        </h3>
      </div>
      <div class="w-full flex flex-row justify-evenly mt-2">
        <SunOutline class="text-white md:w-40 md:h-40 w-32 h-32" />
        <SunFill class="text-gray-200 md:w-40 md:h-40 w-32 h-32" />
      </div>
      <div class="w-full flex flex-row justify-evenly mb-2">
        <p class="text-white md:text-4xl text-2xl mr-2">{{ sunrise }}</p>
        <p class="text-white md:text-4xl text-2xl ml-2">{{ sunset }}</p>
      </div>
    </div>
    <!-- Info about weather -->
    <div class="flex md:flex-col justify-center align-center">
      <figure class="w-1/2 md:w-full">
        <img
          :src="iconSrc"
          class="mx-auto h-full w-1/2 md:h-60 md:w-full object-contain"
        />
      </figure>
      <div class="w-1/2 md:w-full flex justify-evenly md:flex-row flex-col">
        <p class="mt-5 text-4xl font-semibold mb-auto text-center text-white ">
          {{ mainWeather }}
        </p>
        <p class="mt-5 text-4xl font-semibold mb-auto text-center text-white ">
          {{ temperature.val.toFixed(2) }}
          <span v-on:click="convert" class="text-red-500 cursor-pointer">
            {{ temperature.celzius ? "°C" : "°F" }}
          </span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { SunFill, SunOutline } from "../icons";
import countries from "../assets/countries.json";

export default {
  components: {
    SunFill,
    SunOutline,
  },
  name: "WeatherInfo",
  props: ["weatherData"],
  data() {
    return {
      state: countries.filter(
        (country) => country.code === this.weatherData.sys.country
      )[0].name,
      country: this.weatherData.name,
      sunset: new Date(this.weatherData.sys.sunset).toLocaleTimeString("en"),
      sunrise: new Date(this.weatherData.sys.sunrise).toLocaleTimeString("en"),
      iconSrc: this.weatherData.weather[0].icon,
      mainWeather: this.weatherData.weather[0].main,
      temperature: { val: this.weatherData.main.temp, celzius: true },
    };
  },
  methods: {
    convert() {
      if (!this.temperature.celzius) {
        this.temperature = {
          val: (5 / 9) * (this.temperature.val - 32),
          celzius: true,
        };
      } else {
        this.temperature = {
          val: (this.temperature.val * 9) / 5 + 32,
          celzius: false,
        };
      }
    },
  },
};
</script>
