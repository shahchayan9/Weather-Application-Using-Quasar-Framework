<template>
  <q-page class="flex column">
      <div class="col">
        <q-input
          v-model="search"
          @keyup.enter="getWeatherBySearch"
          placeholder="Search"
          borderless
          >
        <template v-slot:before>
          <q-icon
          @click="getLocation"
            name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn
          @click="getWeatherBySearch"
            round
            dense
            flat
            icon="search" />
        </template>
      </q-input>
      </div>

      <template v-if="weatherData">
        <div class="text-black text-center">

        <div class="text-h4 text-weight-dark">
          {{weatherData.name}}
        </div>

        <div class="text-black text-center text-h6 text weight-light">
          {{weatherData.weather[0].main}}
        </div>

        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{Math.round(weatherData.main.temp)}}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>

        </div>

        <div class="col text-center">
          <img :src="`http://openweathermap.org/img/wn/${ weatherData.weather[0].icon }@2x.png`">
        </div>
      </template>

      <template v-else>
        <div class="col column text-center text-black">
          <div class="col text-h2 text-weight-thick">
            Quasar<br>Weather
          </div>
          <q-btn
            @click="getLocation"
            class="col"
            flat
            >
            <q-icon left size="3em" name="my_location" />
            <div>Find My Location</div>
          </q-btn>
        </div>
      </template>

      <div class="col skyline"></div>

  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '59b7ef44b63ae2fe52bd9715c8a54f3d'
    }
  },
  methods: {
    getLocation () {
      // console.log('lat: ', this.lat)
      navigator.geolocation.getCurrentPosition(position => {
        console.log('position: ', position)
        this.lat = position.coords.latitude
        this.lon = position.coords.longitude
        this.getWeatherByCoords()
      })
    },
    getWeatherByCoords () {
      this.$axios(`${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`).then(response => {
        // console.log('response: ', response)
        this.weatherData = response.data
        // console.log('weather: ', this.weatherData)
      })
    },
    getWeatherBySearch () {
      console.log('getWeatherbySearch')
      this.$axios(`${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`).then(response => {
        // console.log('response: ', response)
        this.weatherData = response.data
        // console.log('weather: ', this.weatherData)
      })
    }
  }
}
</script>

<style lang="sass">
  .degree
    top: -45px
  .skyline
    flex: 0 0 150px
    background: url(../statics/skyline.png)
    background-size: contain
    background-position: center bottom
</style>
