<template>
  <div id="weather-app">
    <form @submit.prevent="getWeather">
      <label for="city">City:</label>
      <input type="text" id="city" v-model="city">
      <button type="submit">Get Weather</button>
    </form>
    <div v-if="loading" class="loading">
      <img src="https://media.giphy.com/media/3oEjI6SIIHBdRxXI40/giphy.gif" alt="Loading">
    </div>
    <table v-if="weatherData" class="weather-data">
      <thead>
        <tr>
          <th>Date</th>
          <th>Temperature</th>
          <th>Description</th>
          <th>Condition</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="day in weatherData.list" class="day">
          <td>{{ day.dt_txt }}</td>
          <td>{{ Math.round(day.main.temp) }}°C</td>
          <td>{{ day.weather[0].description }}</td>
          <td>
            <img :src="`https://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`" alt="Weather icon">
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      weatherData: null,
      loading: false,
    }
  },
  methods: {
    async getWeather() {
      try {
        this.loading = true;
        const apiKey = "a905f5082bf7c0f9e7de51bbe714bd00";
        const url = `https://api.openweathermap.org/data/2.5/forecast?q=${this.city}&units=metric&appid=${apiKey}`;
        const response = await fetch(url);
        const data = await response.json();
        this.loading = false;
        this.weatherData = data;
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>