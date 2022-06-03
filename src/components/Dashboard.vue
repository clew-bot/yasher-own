<template>
<NbaScores/>
  <img v-if="!weather"
    @click="getWeather"
    id="weatherIcon"
    src="../assets/svgs/weather-pouring.svg"
    alt="Choose a Weather"
  />
  <div v-else>
  <p id="weather">{{weather}}°</p>
  <p id="weatherName">{{weatherName}}</p>
  </div>
  <transition>
  <h1 v-if="!loading" id="theTime">{{ getDate }}</h1>
  </transition>
  <h3 id="welcomeMessage" @click="consoleme">Good Evening Chad</h3>
  <h3 id="quoteMessage" @click="consoleme">{{ quotes }}</h3>
</template>

<script>
import moment from "moment";
import quotesJson from "../assets/quotes/quotes.json";
import NbaScores from "@/components/NbaScores.vue";
export default {
  name: "Dashboard",
  components: {
    NbaScores
  },
  data() {
    return {
      time: null,
      interval: null,
      quotes: quotesJson[Math.floor(Math.random() * quotesJson.length)].text,
      lat: null,
      lon: null,
      loading: true,
      weather: null,
      now: new Date(),
      weatherName: null,
    };
  },
  methods: {
    consoleme() {
      
    },
    async getWeather() {
      if (!this.lat || !this.lon) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            this.lat = position.coords.latitude;
            this.lon = position.coords.longitude;
          },
          (error) => {
            console.log(error.message);
          }
        );
      }
      try {
        const response = await fetch(
          `${process.env.VUE_APP_WEATHER_URL}${this.lat}&lon=${this.lon}&appid=${process.env.VUE_APP_WEATHER_KEY}${process.env.VUE_APP_WEATHER_URL_UNIT}`
        );
        const {name, main: {feels_like}} = await response.json();
        this.weather = Math.floor(feels_like);
        this.weatherName = name;
        console.log(this.weatherName);
        const item = {
          weather: Math.floor(this.weather),
          weatherName: this.weatherName,
          expiry: this.now.getTime() + 2.16e+7,
        };
        localStorage.setItem("todaysWeather", JSON.stringify(item));

        localStorage.setItem("weather", this.weather);
        console.log(this.weather);
      } catch (error) {
        console.log(error);
      }
    },
  },
  computed: {
    getDate() {
      return moment(this.time).format("h:mm");
    },
  },
  created() {
    this.interval = setInterval(() => {
      this.time = new Date();
    }, 1000);
    setTimeout(() => {
      this.loading = false;
    }, 1000);
    navigator.geolocation.getCurrentPosition(
      (position) => {
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
      },
      (error) => {
        console.log(error.message);
      }
    );
    const localStorageWeather = localStorage.getItem("todaysWeather");
    if (localStorageWeather) {
      this.weather = JSON.parse(localStorageWeather).weather;
      this.weatherName = JSON.parse(localStorageWeather).weatherName;
      const item = JSON.parse(localStorageWeather);
      if (this.now.getTime() > item.expiry) {
        console.log("Removing old weather...")
        localStorage.removeItem("todaysWeather");
      }
    }
  },
};
</script>

<style scoped>



#weather {
  z-index: 2;
  position: absolute;
  top: -25px;
  right: 10px;
  font-size: 2rem;
  font-weight: bold;
  color: rgb(241, 239, 239);
}

#weatherName {
  z-index: 2;
  position: absolute;
  top: 25px;
  right: 13px;
  font-size: 1rem;
  font-weight: bold;
  color: rgb(241, 239, 239);
}

#weatherIcon {
  z-index: 2;
  position: absolute;
  top: 2px;
  right: 20px;
}

#theTime {
  animation: fadein 2s;
  color: rgb(228, 228, 228);
  position: absolute;
  top: 50%;
  right: 50%;
  transform: translate(50%, -100%);
  font-size: 10.5rem;
  z-index: 2;
  pointer-events: none;
}

#quoteMessage {
  animation: fadein 2s;
  color: rgb(228, 228, 228);
  position: absolute;
  bottom: 0%;
  right: 0%;
  /* transform: translate(50%, 100%); */
  font-size: 1.6rem;
  margin-top: 3rem;
  z-index: 2;
}

#welcomeMessage {
  animation: fadein 2s;
  color: rgb(228, 228, 228);
  position: absolute;
  top: 50%;
  right: 50%;
  transform: translate(50%, 110%);
  font-size: 1.6rem;
  margin-top: 3rem;
  z-index: 2;
}

/* Mobile */

@media screen and (max-width: 600px) {
  #theTime {
    font-size: 5rem;
    transform: translate(50%, -170%);
  }
  #quoteMessage {
    font-size: 1.2rem;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.v-enter-active {
  transition: opacity .2s;
}
.v-leave-active {
  transition: opacity 0.2s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;

}
</style>
