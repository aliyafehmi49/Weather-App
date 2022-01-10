<template>
  <div class="weather-app-content">
    <div class="header-wrapper">
      <div class="header-title">
        <h2 class="title">Weather App</h2>
      </div>
      <div class="search-box">
        <form v-on:submit.prevent="getWeather">
          <input
            type="text"
            placeholder="Search city..."
            v-model="searchCity"
            class="search-bar"
          />
        </form>
      </div>
    </div>
    <div class="weather-wrapper" v-if="weather.location && weather.current">
      <div>
        <p class="location">
          {{ weather && weather.location && weather.location.name }},
          {{ weather && weather.location && weather.location.country }}
        </p>
        <p class="date">{{ date }}</p>
      </div>
      <img
        :src="
          weather &&
          weather.current &&
          weather.current.condition &&
          weather.current.condition.icon
        "
        alt=""
      />
      <p class="temp">
        {{ weather && weather.current && weather.current.temp_c }} &deg;C
      </p>
      <p class="weather">
        {{
          weather &&
          weather.current &&
          weather.current.condition &&
          weather.current.condition.text
        }}
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      api_key: "1e296c3635454c60ba575614220501",
      searchCity: "",
      date: "",
      weather: {},
    };
  },

  methods: {
    async getWeather() {
      try {
        const api = `/current.json?q=${this.searchCity}&key=${this.api_key}`;
        const location = await this.$axios.get(api).then((response) => {
          const res = response.data;
          this.weather = res;
          const time = this.weather.location.localtime;
          this.date = new Date(`${time}`);
        });
        return { location };
      } catch (err) {
        alert("something went wrong");
      }
    },
  },
};
</script>

<style>
.weather-app-content {
  margin: 0;
  min-height: 100vh;
  padding: 32px;
  background: linear-gradient(
    177deg,
    rgba(61, 73, 89, 1) 0%,
    rgb(155, 154, 154) 100%
  );
  /* background-color: rgb(151, 151, 151); */
}

.header-title {
  color: #fff;
  display: flex;
  justify-content: center;
}

.title {
  margin: 16px 0;
  font-size: 42px;
  text-align: center;
}

.search-box {
  display: flex;
  justify-content: center;
}

.search-box .search-bar {
  width: 450px;
  margin: 16px 0;
  padding: 10px 22px;
  font-size: 18px;
  border-radius: 8px;
  border: 1px solid rgb(207, 205, 205);
  outline: none;
  background-color: rgb(255, 255, 255, 0.5);
}

.weather-wrapper {
  text-align: center;
}

.location {
  color: #fff;
  margin-bottom: 16px;
  font-size: 38px;
  font-weight: bold;
}

.date {
  color: #fff;
  margin-top: 6px;
  font-size: 18px;
}

.temp {
  margin: 8px 0;
  color: #fff;
  font-size: 38px;
  font-weight: bold;
}

.weather {
  color: #fff;
  font-size: 38px;
  font-weight: bold;
}
</style>