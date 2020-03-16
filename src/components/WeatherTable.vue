<template>
  <div>
    <b-container class="weather-info">
      <b-row  class="justify-content-md-center">
        <b-table
          :fields="fields" 
          :items="items"
          :busy="isBusy"
          head-variant="light"
          response="sm"
        >
          <template v-slot:table-busy>
            <div class="text-center text-danger my-2">
              <b-spinner class="align-middle"></b-spinner>
              <strong>Loading...</strong>
            </div>
          </template>

          <template v-slot:cell(param)="data">
            <b>{{ data.value }}</b>
          </template>
          <template v-slot:cell()="data">
            {{ data.value }}
          </template>
        </b-table>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  name: 'WeathenTable',
  props:['weatherData', 'isBusy'],

  data() {
    return {
    }
  },
  computed: {
    items () {
      return [
        { param: 'Temperature', value: `${this.weatherData.temp} °C` },
        { param: 'Atmospheric pressure', value: `${this.weatherData.pressure} hPa` },
        { param: 'Humidity', value: `${this.weatherData.humidity} %` },
        { param: 'Wind speed', value: `${this.weatherData.windSpeed} meter/sec` },
        { param: 'Wind direction', value: `${this.weatherData.windDeg} °` }
      ]    
    },
    fields () {
      return [
        { key: 'param', label: this.weatherData.city },
        { key: 'value', label: '' }
      ]
    }
  }
}
</script>