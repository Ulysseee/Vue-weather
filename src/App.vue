<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          name="" 
          id="" 
          class="search-bar" 
          placeholder="e.g Paris, London.." 
          v-model="query" 
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <h2 class="location">{{ weather.name }}, {{ weather.sys.country }}</h2>
          <h3 class="date">{{ dateBuilder() }}</h3>          
        </div>

        <div class="weather-box">
            <img v-if="weather.weather[0].main == 'Clear'" src="./assets/sun.png" alt="">
            <img v-if="weather.weather[0].main == 'Clouds'" src="./assets/cloud.png" alt="">
            <img v-if="weather.weather[0].main == 'Rain'" src="./assets/rain.png" alt="">
            <div class="meteo">
              <h2 class="degrees">{{ Math.round(weather.main.temp) }}°</h2>
              <h3 class="weather-desc">{{ weather.weather[0].description }}</h3>
            </div>

          
            
          <div class="additional-info">
            <div class="hi-low div1">{{ Math.round(weather.main.temp_max) }}° <span class="min">min</span></div>
            <div class="hi-low div2">{{ Math.round(weather.main.temp_min) }}° <span class="min">max</span></div>
            <div class="weather div3">{{ Math.round(weather.wind.speed * 3.6) }} km/h <span class="min">Wind</span></div>
            <div class="humidity div4">{{ weather.main.humidity }}% <span class="min">Humidity</span></div>
          </div>
        </div>
      </div>

    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: '334fa07bc9b1226959a974bb8de2392f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {

      }
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
      console.log(results);
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} — ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
  font-family: 'Inter', sans-serif;
}

#app {
  width: 100%;
  /* height: 100vh; */
  background-color: #2270EF;
  color: white;
  transition: all 1s cubic-bezier(.694, .048, .335, 1);
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.00), rgba(0, 0, 0, 0.10));
}

#app.warm {
  background-color: #DB3A34;
  color: #e0e0e0;
}

main {
  max-width: 400px;
  min-height: 100vh;
  padding: 15px 30px;
  position: relative;
  color: inherit;
}

#line {
  width: 100%;
  height: 1.25px;
  background-color: white;
}

#line.warm {
  background-color: black;
}

.weather-wrap {
  width: 100%;
  height: auto;
  margin-top: 5%;

  border-radius: 10px;
  background: rgba(0, 0, 0, 0.15);
  padding: 10px 20px 20px 20px;

  box-shadow: 0px 0px 50px rgba(0, 0, 0, 0.4);
}

.search-box {
  width: 100%;
  /* background-color: inherit; */
  color: inherit;
  
}

.search-box .search-bar {
  font-family: 'Inter', sans-serif;
  display: block;
  width: 100%;
  padding: 16px 22px;

  color: #5e5e5e;
  font-size: 1.25em;
  font-weight: 400;

  appearance: none;
  border: none;
  outline: none;


  box-shadow: 0px 0px 16px 8px rgba(0, 0, 0, .10);
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 12px;
  transition: .4s;
}

::placeholder {
  color: inherit;
  opacity: .8;
}

#search-line {
  width: 100%;
  height: 1.25px;
  background-color: white;
}

#search-line.warm {
  background-color: black;
}

.search-box .search-bar:focus {
  color: #4b4a4a;
  background-color: rgba(255, 255, 255, 0.9);
  box-shadow: 0px 0px 16px 8px rgba(0, 0, 0, .20);
}

.location-box .location {
  color: inherit;
  font-size: 1.75em;
  font-weight: 700;
  /* text-align: center; */
  padding: 5px 0;
  margin: 0 0 5px 0;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: inherit;
  font-size: 1.125em;
  font-weight: 500;
  font-style: italic;
  margin: 5px 0px 20px 0;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  /* text-align: center; */
}

.weather-box {
  height: 68vh;
  text-align: center;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}



img {
  width: 85%;
  height: auto;
  object-fit: contain;
  margin: 0 auto;
  transition: all 1s cubic-bezier(.694, .048, .335, 1);

  filter: drop-shadow(8px 8px 0px rgba(0,0,0,0.25));
}


.meteo {
  width: 70%;
  padding: 5px 15px 15px 15px;
  border-radius: 15px;
  background: rgba(0, 0, 0, 0.10);

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.degrees {
  width: auto;
  margin: 0;
  text-align: left;

  font-size: 6.25em;
  color: inherit;
  font-weight: 900;
  letter-spacing: -2px;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

}

.weather-desc {
  margin: 0;
  text-align: left;
  text-transform: capitalize;
  color: inherit;
  font-size: 1em;
  font-weight: 600;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.2);
}

.additional-info {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;

  background-color:rgba(255, 255, 255, 0.25);

  padding: 10px 15px;
  border-radius: 15px;
  background: rgba(0, 0, 0, 0.10);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.div1 { grid-area: 1 / 1 / 2 / 2; }
.div2 { grid-area: 2 / 1 / 3 / 2; }
.div3 { grid-area: 1 / 2 / 2 / 4; }
.div4 { grid-area: 2 / 2 / 3 / 4; }

.additional-info .hi-low {
  text-align: left;
  color: inherit;
  font-size: 1.25em;
  font-weight: 700;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.2);
  margin: 5px 0;

}

.additional-info .weather {
  text-align: right;
  color: inherit;
  font-size: 1.25em;
  font-weight: 700;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.2);
  margin: 5px 0;
}

.additional-info .humidity {
  text-align: right;
  color: inherit;
  font-size: 1.25em;
  font-weight: 700;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.2);
  margin: 5px 0;
}

.min {
  font-size: .625em;
}

@media only screen and (min-width: 480px) {
  #app {
    display: flex;
    justify-content: center;
  }
}

</style>
