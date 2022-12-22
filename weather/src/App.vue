<template>
  <div id="weather-app">
    <div class="bg-gray-900 py-4">
      <div class="container mx-auto px-4 inline-flex items-center">
        <form class="relative w-64 mx-auto" @submit.prevent="getWeather">
          <label for="city" class="sr-only">City:</label>
          <input type="text" id="city" v-model="city"
            class="input bg-gray-800 rounded-full py-2 px-4 pl-10 text-md text-white focus:outline-none focus:shadow-outline">
          <label for="display-option" class="sr-only">Display Option:</label>
          <select id="display-option" v-model="displayOption" class="select bg-gray-800 rounded-full py-2 px-4 pl-10 text-md text-white focus:outline-none focus:shadow-outline">
            <option value="hourly">Every 4h</option>
            <option value="daily">Daily</option>
          </select>
          <button type="submit" class="bg-gray-600 rounded-full py-2 px-4 text-md text-white hover:bg-gray-700">Get
            Weather</button>
        </form>
      </div>
    </div>
    <!-- Use the displayOption variable to determine which data to display -->
    <table v-if="hourlyData[0] || dailyData[0]" class="weather-data w-full text-left table-collapse py-4 px-4">
      <thead class="bg-gray-800">
        <tr class="text-white">
          <th class="py-2 px-4">Time</th>
          <th class="py-2 px-4">Temperature</th>
          <th class="py-2 px-4">Description</th>
          <th class="py-2 px-4">Condition</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="displayOption === 'hourly' && hourlyData" v-for="day in hourlyData.list" class="day" :key="day.dt_txt">
          <td class="py-2 px-4">{{ day.dt_txt }}</td>
          <td class="py-2 px-4">{{ Math.round(day.main.temp) }}°C</td>
          <td class="py-2 px-4">{{ day.weather[0].description }}</td>
          <td class="py-2 px-4">
            <img :src="`https://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`" alt="Weather icon">
          </td>
        </tr>
        <tr v-if="displayOption === 'daily' && dailyData" v-for="day in dailyData" class="day" :key="day.dt_txt">
          <td class="py-2 px-4">{{ day.dt_txt }}</td>
          <td class="py-2 px-4">{{ Math.round(day.main.temp) }}°C</td>
          <td class="py-2 px-4">{{ day.weather[0].description }}</td>
          <td class="py-2 px-4">
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
      dailyData: [],
      hourlyData: [],
      loading: false,
      displayOption: "daily",
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
        // Split the data into daily and hourly intervals
        this.dailyData = data.list.filter((item, index) => index % 8 === 0);
        this.hourlyData = data;
      } catch (error) {
        console.error(error);
        this.weatherData = null;
        this.dailyData = [];
        this.hourlyData = [];
      }
    }
  }
};
</script>