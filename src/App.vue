<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>Check the weather in {{ cityName }}</p>
    <input type="text" v-model="city" placeholder="Enter city" />
    <button :disabled="!isCityValid" @click="getWeather">Get Weather</button>
    <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
    <div v-if="weatherData">
      <p>Current temperature: {{ showTemp }}&deg;C</p>
      <p>Feels like: {{ showFeelsLike }}&deg;C</p>
      <p>Minimum temperature: {{ showMinTemp }}&deg;C</p>
      <p>Maximum temperature: {{ showMaxTemp }}&deg;C</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      errorMessage: '',
      weatherData: null,
    };
  },
  computed: {
    cityName() {
      return this.city ? `<<${this.city}>>` : 'your city';
    },
    isCityValid() {
      return this.city.trim().length >= 2;
    },
    showTemp() {
      return this.weatherData?.temp;
    },
    showFeelsLike() {
      return this.weatherData?.feels_like;
    },
    showMinTemp() {
      return this.weatherData?.temp_min;
    },
    showMaxTemp() {
      return this.weatherData?.temp_max;
    },
  },
  methods: {
    async getWeather() {
      try {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=43afafc4ef511b02e0e1bab76b0a5cac`
        );
        this.weatherData = response.data?.main;
        this.errorMessage = '';
      } catch (error) {
        this.errorMessage =
          error.response?.data?.message || 'An error occurred while fetching weather data.';
        this.weatherData = null;
      }
    },
  },
};
</script>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  padding: 20px;
  border-radius: 50px;
  text-align: center;
  color: #FFF;
  background: #1f0f24;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper button {
  background: #e3bc4b;
  border-radius: 10px;
  color: #FFF;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:disabled {
  background: #e3bc4b;
  opacity: 0.3;
  cursor: not-allowed;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: none;
  border-bottom: 2px solid #110813;
  color: #FCFCFC;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.error {
  color: red;
}
</style>
