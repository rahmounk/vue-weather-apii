<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/meteo.png" width="250px" />
    <h1>WE ZEURE APP</h1>
    <form class="search-location" @submit.prevent="getWeather">
      <input
        type="text"
        placeholder="search..."
        v-model="citySearch"
        autocomplete="off"
      />
    </form>

    <div ID="weatherdesc">
            <p>{{ weather.temperature }}</p>
          </div>

          <div class="weather-container widget" :class="isDay ? 'day' : 'night'">
      <div class="weather-box" v-if="searchResult">
        <div ID="cloudcontainer">
          <div ID="cloudmain">
            <div ID="cloud2"></div>
            <div ID="cloud3"></div>
          </div>
        </div>
        <div ID="sun"></div>
        <div ID="weatherdescboxes">
          <div ID="weatherdesc">
            <p>{{ weather.temperature }}</p>
          </div>
          <div ID="weatherdesc2">
            <h2 ID="name">{{ weather.cityName }}</h2>
            <p ID="datedesc"> {{ weather.description }} </p>

            <p class="weathmore">low : <span>{{ weather.lowTemp }}</span>°C</p>
            <p class="weathmore">max : <span>{{ weather.highTemp }}</span> °C</p>
            <p class="feels">feel : <span>{{ weather.feelsLike }}</span></p>
          </div>
        </div>
      </div>

      <!-- ne pas afficher la météo si la ville n'est pas trouvée -->
      <div class="weather-box" v-else>City Not found</div>
    </div>

    
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      
      isDay: true,
      searchResult: false,
      citySearch: "",
      weather: {
        cityName: "Bordeaux",
        country: "FR",
        temperature: 9,
        description: "Cloudy with a chance of meatballs",
        lowTemp: 0,
        highTemp: 15,
        feelsLike: "Death",
        humidity: 55,
      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      const key = "c5e05433f88e224b12600efdc4ab7be5";
      const callURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      //? APPEL A l'API avec un await dans un try
      try {
        const response = await fetch(callURL);
        const data = await response.json(response);
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);

        this.searchResult = true;

        //  Check if it's day or night
        const dayNight = data.weather[0].icon;

        if (dayNight.includes("d")) {
          this.isDay = true;
        } else {
          this.isDay = false;
        }
      } catch (error) {
        console.log(error);
        this.searchResult = false;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

* {
  margin: 0;
  padding: 0;
  outline: none;
}

.night {
  background-color: midnightblue;
}

input {
  border: 1px solid red;
  border-radius: 10px;
  margin-top: 5px;
  padding: 5px;
}

body {
  background-color: #0a84e3;
  font-family: "Raleway", sans-serif;
}

span {
  color: rgb(42, 158, 158);
  font-weight: 600;
  
}

.weather-container {
  display: flex;
  justify-content: space-around;
  flex-direction: row;
}

.weather-box {
  top: 45%;
  width: 450px;
  position: absolute;
  background-color: #0985e3c5;
  background: rgb(18, 159, 241);
  background: linear-gradient(
    180deg,
    rgba(18, 159, 241, 1) 24%,
    rgba(52, 244, 249, 1) 100%
  );
  border-radius: 20px;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1), 0 3px 60px 0 rgba(0, 0, 0, 0.19);
  display: flex;
  flex-direction: column;
}

#weatherdescboxes {
  display: flex;
  background-color: #ffffff;
}

#weatherdesc {
  position: relative;
  height: 100px;
  background-color: transparent;
  font-size: 4.5em;
  font-weight: normal;
  color: #0a84e3;
  width: 50%;
  margin-top: 15px;
  padding-left: 30px;
}

#weatherdesc p {
  margin-left: 30px;
}

#weatherdesc2 {
  height: 100px;
  font-size: 0.9em;
  color: #0b71bf;
  background-color: transparent;
  border: 0px solid red;
  width: 50%;
  margin-top: 10px;

}

p {
  margin-top: 0px;
  margin-left: 20px;
}

#datedesc {
  color: #505050;
  font-weight: bold;
  font-size: 1.1em;
}

.weathmore:nth-child(2) {
  font-weight: bold;
}

.weathmore {
  padding-top: 0px;
  margin-bottom: 0px;
}

#sun {
  left: 25%;
  position: relative;
  height: 200px;
  width: 200px;
  border-radius: 50%;
  margin: 50px;
  background-color: #ffca00;
  box-shadow: 0 0 0 3px #fee990, 0 0 50px #ffca00, 0 0 50px #ffa500;
  animation: sunshine 4s infinite linear alternate-reverse;
}

@keyframes sunshine {
  0% {
    box-shadow: none;
  }
}

#cloudcontainer {
  z-index: 1;
  position: absolute;
  top: 45%;
  right: 120px;
  animation: cloudfloating 12s infinite ease-in-out alternate;
}

@keyframes cloudfloating {
  0% {
    left: 30%;
  }

  25% {
    left: 20%;
  }

  50% {
    left: 25%;
  }

  75% {
    left: 20%;
  }

  100% {
    left: 30%;
  }
}

#cloudmain {
  position: relative;
  width: 160px;
  height: 80px;
  border-radius: 50%;
  background: #e9e9e9;
  opacity: 90%;
}

#cloud2 {
  position: absolute;
  top: -15px;
  left: 80px;
  background: #e9e9e9;
  width: 50px;
  height: 70px;
  border-radius: 50%;
}

#cloud3 {
  position: absolute;
  top: -20px;
  left: 30px;
  background: #e9e9e9;
  width: 65px;
  height: 90px;
  border-radius: 50%;
}

.night {
  background-color: midnightblue;
}

</style>
