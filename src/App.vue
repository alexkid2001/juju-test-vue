<template>
  <div id="app">
    <b-container>
      <b-row class="justify-content-md-center">
        <b-col cols="12" md="auto">
          <b-button-group>
            <b-button variant="outline-primary" @click="getWeather(kyivID, $event)">
              <b-spinner small v-if="isBusy"></b-spinner>
              Kyiv
            </b-button>
            <b-button variant="outline-primary" @click="getWeather(londonID, $event)">
              <b-spinner small v-if="isBusy"></b-spinner>
              London
            </b-button>
            <b-button variant="outline-primary" @click="getWeather(nyID, $event)">
              <b-spinner small v-if="isBusy"></b-spinner>
              New York
            </b-button>
          </b-button-group>
        </b-col>
      </b-row>
    </b-container>
    <WeatherTable 
      :weatherData="weatherData" 
      :isBusy="isBusy"
    />
  </div>
</template>

<script>
import WeatherTable from './components/WeatherTable'
import axios from 'axios'
import './styles/common.scss'

const key = 'c2dcf8ffb5cdc3f8977bfd2ae7ea4738';
const baseUrl = 'https://api.openweathermap.org/data/2.5/weather';

export default {
  name: 'app',
  data () {
    return {
      weatherData: {
        city: '',
        temp: '',
        humidity: '',
        pressure: '',
        windSpeed: '',
        windDeg: '',
      },
      isBusy: true,
      kyivID: '703448',
      londonID: '2643743',
      nyID: '5128638',
    }
  },
  components: {
    WeatherTable
  },
  methods: {
    getWeather (cityID, $event) {
      this.toggleCurrent();
      if ($event) {
        $event.target.classList.remove('btn-outline-primary');
        $event.target.classList.add('btn-primary');
      }
       
      axios({
        method: 'get',
        baseURL: baseUrl,
        params: {
          id: cityID,
          appid: key,
          units: 'metric'
        }
      })
        .then(resp => {
          this.weatherData.temp = resp.data.main.temp;
          this.weatherData.humidity = resp.data.main.humidity;
          this.weatherData.pressure = resp.data.main.pressure;
          this.weatherData.windSpeed = resp.data.wind.speed;
          this.weatherData.windDeg = resp.data.wind.deg;
          this.weatherData.city = resp.data.name;
          this.isBusy = false;
        })
        .catch(err => console.log(err));
    },
    toggleCurrent () {
      const buttons = document.querySelectorAll('.btn');
      buttons.forEach((btn) => {
        if ( btn.classList.contains('btn-primary') ) {
          btn.classList.remove('btn-primary');
          btn.classList.add('btn-outline-primary');
        }
      })
    }
  },
  mounted () {
    this.getWeather(this.kyivID);
  }
}
</script>

<style lang="scss">
</style>
