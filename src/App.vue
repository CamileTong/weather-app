<template>
  <div id="app"
        :class="typeof weatherR.main != 'undefined' && weatherR.main.temp > 16 ? 'warm' : ''">
    <main>
      <input class="search"
              type="text"
              placeholder="Search..."
              v-model="city"
              @keyup.enter="fetchWeather">
      <div class="weather-wrap"
            v-if="typeof weatherR.main != 'undefined'">
        <h2 class="city">{{weatherR.name}}, {{weatherR.sys.country}}</h2>
        <p class="date">{{dateBuilder()}}</p>
        <p class="temperature">{{Math.round(weatherR.main.temp)}}°C</p>
        <p class="weather">{{weatherR.weather[0].description}}</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: 'a587ae74e89865a7afbdf8bb2b824ae8',
      url_base: "https://api.openweathermap.org/data/2.5/",
      city: '',
      weatherR: {},
    }
  },
  methods: {
    // Fetch and update weather of typed in city
    fetchWeather() {
      fetch(`${this.url_base}weather?q=${this.city}&units=metric&appid=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
    },
    setResults (result) {
      this.weatherR = result;
    },

    // Returns date with Day-Date-Month-Year formatt for clear representation
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  font-family: 'montserrat', sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-image: url('./assets/cold-bg.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpeg');
}

main {
  min-height: 100vh;
  padding: 25px;
  margin: 0 auto;
  max-width: 50vh;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.2), rgba(0,0,0,0.7));
  
  display: flex;
  flex-flow: column wrap;
  align-items: center;
}

.search {
  width: 100%;
  color: #928a8a;
  border: none;
  padding: 30px;
  border-radius:20px 0 20px 0;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  font-size: 30px;
  font-weight: lighter;
  outline: none;
}

/* add focus effect */
.search:focus {
  border-radius: 0 20px 0 20px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  outline: none;
}

h2, p {
  color: white;
  margin: 5px;
  text-align: center;
}

.city, .temperature, .weather {
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.city {
  font-size: 40px;
  font-weight: normal;
  margin-top: 30px;
  
}

.date {
  font-size: 25px;
  font-weight: lighter;
  font-style: italic;
}

.temperature {
  font-size: 90px;
  font-weight: bolder;
  padding: 10px;
  margin: 50px auto;
  width: 260px;
  max-width: 260px;

  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 10%;
}

.weather {
  font-size: 50px;
  font-weight: bold;
  font-style: italic;
  text-transform: capitalize;
}
</style>

/* import font */
<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');
</style>
