<template>
  <div id="app" :class="weather.main && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="weather.main">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>

          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "6460aa4ce68f39abdb96c7b064f07fd3",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 2em;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 2em;
}
.search-bar {
  display: block;
  width: 100%;
  padding: 1em;

  color: #313131;
  font-size: 1em;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 0.5em rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 1em 0 1em;
  transition: 0.4s;
}
.search-bar:focus {
  box-shadow: 0 0 1em rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 1em 0 1em 0;
}
.location {
  color: #fff;
  font-size: 2em;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.date {
  color: #fff;
  font-size: 1.2em;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.temperature {
  color: #fff;
  font-size: 5em;
  text-align: center;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 0.7em;
  margin: 1em 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather {
  color: #fff;
  text-align: center;
  font-size: 4em;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
