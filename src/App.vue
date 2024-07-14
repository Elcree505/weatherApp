<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" 
        class="search-bar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
        
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp - 273.15) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const apiKey = 'de02deaf76cd7bbe973660d3da9c3498';
const urlBase = 'https://api.openweathermap.org/data/2.5/';
const query = ref('');
const weather = ref({});

const fetchWeather = (e) => {
  if (e.key === "Enter") {
    fetch(`${urlBase}weather?q=${query.value}&appid=${apiKey}`)
      .then(res => res.json())
      .then(setResults);
  }
};

const setResults = (results) => {
  weather.value = results;
};

const dateBuilder = () => {
  const d = new Date();

  const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
  const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

  const day = days[d.getDay()];
  const date = d.getDate();
  const month = months[d.getMonth()];
  const year = d.getFullYear();

  return `${day} ${date} ${month} ${year}`;
};
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  background: linear-gradient(135deg, #6e8efb, #a777e3);
  min-height: 100vh;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

main {
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 40px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  width: 100%;
}

.search-box {
  margin-bottom: 30px;
}

.search-box .search-bar {
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 18px;
  border: none;
  outline: none;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 25px;
  transition: 0.4s;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.search-box .search-bar:focus {
  background: rgba(255, 255, 255, 1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 600;
  text-align: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 10px;
}

.location-box .date {
  color: #FFF;
  font-size: 18px;
  font-weight: 400;
  font-style: italic;
  text-align: center;
  margin-bottom: 30px;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 20px 30px;
  color: #FFF;
  font-size: 80px;
  font-weight: 900;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  background: rgba(255, 255, 255, 0.3);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.weather-box .weather {
  color: #FFF;
  font-size: 36px;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
}
</style>
