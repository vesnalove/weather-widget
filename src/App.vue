<template>
  <div class="app" :class="typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <div class="wrapper">
      <div class="search_content">
        <input type="text" class="search_bar" placeholder="Search" v-model="query" @keypress="fetchWeather">
      </div>

      <div class="weather_content" v-if="typeof weather.main !== 'undefined'">
        <div class="location_desc">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather_desc">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="feel">Feels like {{ weather.main.feels_like }}°c, {{weather.weather[0].description }}.</div>
        </div>
        <div class="weather_props">
          <div class="humidity">Hymidity: {{ weather.main.humidity }}%</div>
          <div class="visibility">Visibility: {{ weather.visibility / 1000 }}km.</div>
          <div class="speed">Wind speed: {{ weather.wind.speed }}m/s.</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        api_key: 'f070d67cbb630c5cb891f8d31dae2a3c',
        url_base: "https://api.openweathermap.org/data/2.5/",
        query: '',
        weather: {}
      }
    },
    methods: {
      fetchWeather(e) {
        if(e.key == "Enter") {
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res =>  {
            return res.json();
          }).then(this.setResults).then(this.setLocalWeather);
        }
      },
      setResults(results) {
        this.weather = results;
      },
      setLocalWeather() {
        localStorage.query = this.query;
      },
      dateBuilder() {
        let d = new Date();
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];

        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month} ${year}`;
      }
    },
    mounted() {
      if (localStorage.query) {
        this.query = localStorage.query;
      }
    }
  }
</script>

<style lang="scss">
  .app{
    background-image: url('./assets/anime-cold.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }
  .app.warm{
    background-image: url('./assets/anime-warm.jpg');
  }
  .wrapper{
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  .search_content {
    width: 100%;
    margin-bottom: 30px;
    .search_bar{
      display: block;
      width: 100%;
      padding: 15px;

      color: #fff;
      font-size: 20px;

      appearance: none;
      border: none;
      outline: none;
      background: none;
      
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.5);
      border-radius: 16px 16px 16px 16px;
      transition: 0.4s
    }
    .search_bar:focus{
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px 16px 0px;
    }
  }

  .location_desc{
    .location{
      color: #fff;
      font-size: 32px;
      font-weight: 500;
      text-align: center;
      text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }
    .date{
      color: #fff;
      font-size: 20px;
      font-weight: 300;
      font-style: italic;
      text-align: center;
    }
  }

  .weather_desc{
    text-align: center;

    .temp{
      display: inline-block;
      padding: 10px 25px;
      color: #fff;
      font-size: 102px;
      font-weight: 900;

      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.25);
      border-radius: 16px;
      margin: 30px 0px;

      box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
    .weather{
      color: #fff;
      font-size: 48px;
      font-weight: 700;
      font-style: italic;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
    .feel {
      color: #fff;
      font-size: 18px;
      margin-bottom: 15px;
    }
  }
  .weather_props{
    text-align: center;
    color: #fff;
    padding: 5px;
    .humidity{
      margin-bottom: 5px;
    }
    .visibility{
      margin-bottom: 5px;
    }
  }
</style>
