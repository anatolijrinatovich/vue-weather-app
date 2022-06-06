<template>
  <div id="app">
    <b-navbar toggleable type="dark" variant="dark">
      <b-navbar-brand style="margin-left: 5%;" href="#">Weather forecast Apps</b-navbar-brand>

    </b-navbar>
    <div class="map container" style="max-width: 95%">
      <!--======  map component  ==========-->
      <l-map
          :zoom="10"
          :center="center"
          style="height: 50vh; position: relative;"
          @click="mapClick"
      >
        <l-tile-layer
            :url="mapLink"
            atribution="some text here"
        >
        </l-tile-layer>
        <!--      checking v-if coordinate is unevenly undefined-->
        <l-marker
            v-if="coord != undefined"
            ref="marker"
            :lat-lng="getCoord()"
        >
          <!--  binding data to an attribute using the v-bind,
           the transmitted data from the object (coord) with its elements      -->
          <l-icon :icon-url="coord.icon" :icon-size="coord.iconSize"></l-icon>
          <l-popup :content="coord.content"></l-popup>
        </l-marker>
      </l-map>
      <div class="small_text">
        <small class="text-muted">Click on the map to get exact coordinates. (Our input them manually on the fields
          bellow)</small>
      </div>

      <div class="form container">
        <div class="coordinate">
          <div>
            <!--  latitude is input using the v-model directive for two-way data binding of the input field -->
            <!--  v-on:change event calls the mapClick() function which contains the getCord()  -->
            <!-- function which accepts the coordinate drag object  -->
            <!--          when changing the data in the input, the marker moved on the map-->
            <b-form-input type="number" step="0.0001" name="latitude" id="latitude" placeholder="Latitude"
                          v-model="coord.lat"
                          @change="mapClick({latlng: getCoord()})"></b-form-input>
          </div>

          <div>
            <!--  latitude is input using the v-model directive for two-way data binding of the input field -->
            <!--  v-on:change event calls the mapClick() function which contains the getCord()  -->
            <!-- function which accepts the coordinate drag object  -->
            <!--          when changing the data in the input, the marker moved on the map-->
            <b-form-input type="number" step="0.0001" name="longitude" id="longitude" placeholder="Longitude"
                          v-model="coord.long"
                          @change="mapClick({latlng: getCoord()})"></b-form-input>
          </div>
          <div class="geo-icon">
            <b-icon class="icon  rounded p-2" icon="geo-alt" font-scale="3" variant="secondary"
                    @click="getLocationGPS"></b-icon>
          </div>
        </div>
      </div>
      <!--    components chart.js-->
      <!--    redraw each time if the response is empty-->
      <Chart v-if="response != undefined" :chartData="response" :chartType="showDaily"></Chart>


      <div class="card text-center">
        <div class="card-header">
          Settings
        </div>
        <div class="card-body">
          <div class="current-checkbox">
            <div class="form-check form-switch">
              <input v-model="settings.current" class="form-check-input" type="checkbox" id="current_weather"
                     name="current_weather"
                     value="true"
                     @change="requestToApi(getCoord(coord.lat,coord.long))">
              <label for="current_weather" class="form-check-label">Current weather data (Marker dynamic icons)</label>
            </div>

<!--            container select (setting) configures the network setup-->
          </div>
          <div class="settings_containers">
            <div class="col-3">
              <div>
                <b-form-select v-model="settings.temperature" :options="temperature" name="temperature_unit"
                               id="temperature_unit"
                               aria-label="Temperature Unit" data-default="celsius"
                               @change="requestToApi(getCoord(coord.lat,coord.long))">
                </b-form-select>
              </div>
            </div>
<!--            Settings select-->
            <div class="col-3">
              <div>
                <b-form-select v-model="settings.wind" :options="wind" name="windspeed_unit" id="windspeed_unit"
                               aria-label="Windspeed Unit"
                               data-default="kmh"
                               @change="requestToApi(getCoord(coord.lat,coord.long))">
                </b-form-select>
              </div>
            </div>
            <div class="col-3">
              <div>
                <b-form-select v-model="settings.precipitation" :options="precipitation" name="precipitation_unit"
                               id="precipitation_unit"
                               aria-label="Precipitation Unit" data-default="mm"
                               @change="requestToApi(getCoord(coord.lat,coord.long))">
                </b-form-select>
              </div>
            </div>
            <div class="col-3">
              <div>
                <b-form-select v-model="settings.timeformat" :options="timeformat" name="timeformat" id="timeformat"
                               aria-label="Timeformat"
                               data-default="iso8601" @change="requestToApi(getCoord(coord.lat,coord.long))">
                </b-form-select>
              </div>
            </div>
            <div class="col-3">
              <div>
                <b-form-select v-model="settings.timezone" :options="timezone" name="timezone" id="timezone"
                               aria-label="Timezone"
                               data-default="UTC"
                               @change="requestToApi(getCoord(coord.lat,coord.long))">
                </b-form-select>
              </div>
            </div>
            <div class="col-3">
              <div>
                <b-form-select v-model="settings.past" :options="past" name="past_days" id="past_days"
                               aria-label="Past days" data-default="0"
                               @change="requestToApi(getCoord(coord.lat,coord.long))">
                </b-form-select>
              </div>
            </div>
          </div>
        </div>

      </div>
      <div style="margin-top: 2rem; text-align: center;" class="card text-muted">
        <h5>API URL (
          <b-link :href="weatherLink" target="_blank">Open in new tab</b-link>
          )
        </h5>
        <!--    the input field in which the API link is located,
        using the directive to the two-way binding model,
        we pass the value of the variable (weatherLink) input parameters-->
        <b-form-input class="weatherLink" type="text" rel="link" readonly="readonly"
                      v-model="weatherLink"></b-form-input>
      </div>
      <div class="small_text"><small class="text-muted">You can copy this API URL into your application.</small></div>

      <div class="weather_variables">
        <h3>Hourly Weather Variables</h3>
        <div class="hourly_container">
          <!--        iterate over the daily array, sorting through item and index, as key value index and item-->
          <div class="hourly" v-for="(item, index) in hourly" :key="index + item">
            <!--        renders each checkbox, binds value to item , id sets as index + item-->
            <input type="checkbox" :value="item" :id="index + item" name="hourly" @change="selectHourly(item)">
            <!--        for this id, we rewrite the label and display it on the page -->
            <label :for="index + item" class="label-margin">
              <!--          remove the underscore and leave an indent-->
              {{ item.replaceAll('_', ' ') }}
            </label>
          </div>
        </div>

        <h3>Daily Weather Variables</h3>
        <div class="daily_container">
          <!--        iterate over the daily array, sorting through item and index, as key value index and item-->
          <div v-for="(item, index) in daily" :key="index + item">
            <!--        renders each checkbox, binds value to item , id sets as index + item-->
            <input class="hourly" type="checkbox" :value="item" :id="index + item" name="daily"
                   @change="selectDaily(item)">
            <!--        for this id, we rewrite the label and display it on the page -->
            <label :for="index + item" class="label-margin">
              <!--          remove the underscore and leave an indent-->
              {{ item.replaceAll('_', ' ') }}
            </label>
          </div>
        </div>
        <div style="margin-top: 1rem;margin-bottom: 1.5rem">
          <small  class="text-muted">* Parameter timezone is mandatory</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

// Import Bootstrap and BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'


// import function which provides a latitude and longitude object
// eslint-disable-next-line no-unused-vars
import {latLng, Icon} from 'leaflet';

// imported map components
import {LMap, LTileLayer, LMarker, LPopup, LIcon} from 'vue2-leaflet';
import Chart from './components/chart.vue';

export default {
  name: 'App',
  components: {
    // connected map components
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LIcon,
    Chart
  },
  // function data return object with variables
  data() {
    return {

      // variables, link to API
      weatherLink: 'https://api.open-meteo.com/v1/forecast?',
      // variables, link to leaflet map API
      mapLink: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      // array of pictures
      images: [
        {
          code: [0],
          path: 'img/wi-day-sunny.b7e1ba13.svg'
        },
        {
          code: [1, 2, 3],
          path: 'img/wi-day-sunny-overcast.7d533f2e.svg'
        },
        {
          code: [45, 48],
          path: 'img/wi-fog.57a5cbcb.svg'
        },
        {
          code: [51, 53, 55],
          path: 'img/wi-rain.44130c44.svg'
        },
        {
          code: [56, 57],
          path: 'img/wi-rain-mix.c47a0eef.svg'
        },
        {
          code: [61, 63, 65],
          path: 'img/wi-rain-mix.c47a0eef.svg'
        },
        {
          code: [66, 67],
          path: 'img/wi-rain-mix.c47a0eef.svg'
        },
        {
          code: [71, 73, 75],
          path: 'img/wi-showers.072fe053.svg'
        },
        {
          code: [77],
          path: 'img/wi-snow.b215ef27.svg'
        },
        {
          code: [80, 81, 82],
          path: 'img/wi-snowflake-cold.babbd220.svg'
        },
        {
          code: [85, 86],
          path: 'img/wi-snow-wind.c9d31909.svg'
        },
        {
          code: [95],
          path: 'img/wi-storm-showers.152d8c14.svg'
        },
        {
          code: [96, 99],
          path: 'img/wi-thunderstorm.59ea23f8.svg'
        },
      ],
      // array of day variables
      daily: [
        'temperature_2m_max\n',
        'temperature_2m_min',
        'apparent_temperature_max\n',
        'apparent_temperature_min',
        'precipitation_sum',
        'precipitation_hours',
        'weathercode',
        'sunrise\n',
        'sunset',
        'windspeed_10m_max\n',
        'windgusts_10m_max',
        'winddirection_10m_dominant',
        'shortwave_radiation_sum'

      ],
      // array of clock variables
      hourly: [
        'temperature_2m',
        'relativehumidity_2m',
        'dewpoint_2m',
        'apparent_temperature',
        'pressure_msl',
        'cloudcover',
        'cloudcover_low',
        'cloudcover_mid',
        'cloudcover_high',
        'windspeed_10m\n',
        'windspeed_80m\n',
        'windspeed_120m\n',
        'windspeed_180m',
        'winddirection_10m\n',
        'winddirection_80m\n',
        'winddirection_120m\n',
        'winddirection_180m',
        'windgusts_10m',
        'shortwave_radiation',
        'direct_radiation\n',
        'direct_normal_irradiance',
        'vapor_pressure_deficit',
        'evapotranspiration',
        'precipitation',
        'weathercode',
        'snow_depth',
        'freezinglevel_height',
        'soil_temperature_0cm\n',
        'soil_temperature_6cm\n',
        'soil_temperature_18cm\n',
        'soil_temperature_54cm',
        'soil_moisture_0_1cm\n',
        'soil_moisture_1_3cm\n',
        'soil_moisture_3_9cm\n',
        'soil_moisture_9_27cm\n',
        'soil_moisture_27_81cm'
      ],
      // response API object
      response: undefined,
      // coordinate default object
      coord: {
        id: 1,
        lat: '',
        long: '',
        icon: require('leaflet/dist/images/marker-icon-2x.png'),
        iconSize: [25, 41],
        content: ''
      },
      // Initial coordinates. Align the map to the center after clicking ====== variables
      center: latLng(56.8796, 24.6032),
      // network settings object
      settings: {
        // hour settings with empty array variables
        hourly: [],
        // day settings with empty array variables
        daily: [],
        // to show the current weather on the map variables
        current: true,
        // temperature display in Celsius variables
        temperature: 'celsius',
        // wind speed km per hour variables
        wind: 'kmh',
        // pressure in millimeters variables
        precipitation: 'mm',
        // temporary format variables
        timeformat: 'iso8601',
        // time zone variables
        timezone: 'UTC',
        // last day variables
        past: 0,
      },
      // setting temperature Array
      temperature: [
        {value: 'celsius', text: 'Celsius °C'},
        {value: 'fahrenheit', text: 'Fahrenheit °F'}

      ],
      // setting wind Array
      wind: [
        {value: 'kmh', text: 'Km/h'},
        {value: 'ms', text: 'm/s'},
        {value: 'mph', text: 'Mph'},
        {value: 'kn', text: 'Knots'}

      ],
      // setting precipitation Array
      precipitation: [
        {value: 'mm', text: 'Milimeter'},
        {value: 'inch', text: 'Inch'}
      ],
      // setting timeformat Array
      timeformat: [
        {value: 'iso8601', text: 'ISO 8601 (e.g. 2021-01-01)'},
        {value: 'unixtime', text: 'Unix timestamp'}
      ],
      // Setting timezone Array
      timezone: [
        {value: 'UTC', text: 'UTC'},
        {value: 'America/Adak', text: 'America/Adak'},
        {value: 'America/Los_Angeles', text: 'America/Los_Angeles'},
        {value: 'America/Anchorage', text: 'America/Anchorage'},
        {value: 'America/Denver', text: 'America/Denver'},
        {value: 'America/Chicago', text: 'America/Chicago'},
        {value: 'America/New_York', text: 'America/New_York'},
        {value: 'America/Sao_Paulo', text: 'America/Sao_Paulo'},
        {value: 'America/Argentina/Mendoza', text: 'America/Argentina/Mendoza'},
        {value: 'America/Argentina/Cordoba', text: 'America/Argentina/Cordoba'},
        {value: 'Europe/London', text: 'Europe/London'},
        {value: 'Europe/Berlin', text: 'Europe/Berlin'},
        {value: 'Europe/Moscow', text: 'Europe/Moscow'},
        {value: 'Africa/Cairo', text: 'Africa/Cairo'},
        {value: 'Asia/Bangkok', text: 'Asia/Bangkok'},
        {value: 'Asia/Singapore', text: 'Asia/Singapore'},
        {value: 'Asia/Tokyo">Asia', text: 'Asia/Tokyo">Asia'},
        {value: 'Australia/Sydney', text: 'Australia/Sydney'},
        {value: 'Pacific/Auckland', text: 'Pacific/Auckland'}
      ],
      // Setting past Array
      past: [
        {value: '0', text: '0'},
        {value: '1', text: '1'},
        {value: '2', text: '2'}

      ],

    }
  },
  // lifecycle hook
  mounted() {

  },
  methods: {
    // coordinate comes with (item.latlng)
    requestToApi(coord) {
      this.response = undefined
      // bring the coordinate to the object and forward
      // console.log(coord)
      this.coord = {
        // some elements
        id: 1,
        lat: coord.lat,
        long: coord.lng,
        icon: require('leaflet/dist/images/marker-icon-2x.png'),
        iconSize: [25, 41],
        content: ''
      }
      //get data over json network
      fetch(this.constructWeatherLink())
          .then(response => response.json())
          .then((json) => {
            //equate our json to the response
            this.response = json
            // console.log(json)
            // create a variable in which we write the content
            let content = ''
            // if not equal to undefined then passes the test
            if (json.current_weather != undefined) {
              // and writes the received data to a variable
              content += `
                <h2><b>Current Status</b></h2>
               <b>Time:</b> ${json.current_weather.time.replace('T', ' ')} ${this.settings.timezone}
               <br><b>Temperature:</b> ${json.current_weather.temperature} ${this.settings.temperature}
               <br><b>Wind Direction:</b> ${json.current_weather.winddirection}
               <br><b>Wind Speed:</b> ${json.current_weather.windspeed} ${this.settings.wind}`
            }
            // check if our hourly are not empty
            if (json.hourly != undefined) {
              // create inscription 'Hourly'
              content += '<h2><b>Hourly</b></h2>'
              // we start the loop and run through the selected array of our values
              for (let i = 0; i < this.settings.hourly.length; i++) {
                // and we are looking for our alimony [i] in 'json.hourly' and if found, return the value
                if (json.hourly[this.settings.hourly[i]] != undefined) {
                  // write the name of this element [i] into the content, remove all underscores from the element [i]
                  content += `<b>${this.settings.hourly[i].replaceAll('_', ' ')}:</b>
                    <!--  we take the getMiddleValue function, pass the hour value [i], pass current time "current_weather" and an array time json.hourly[time] there                      -->
                  ${this.getMiddleValue(json.hourly[this.settings.hourly[i]], json.current_weather.time, json.hourly['time'])}<br>`
                }
              }
            }
            // check if our daily are not empty
            if (json.daily != undefined) {
              // create inscription 'Hourly'
              content += '<h2><b>Daily</b></h2>'
              // we start the loop and run through the selected array of our values
              for (let i = 0; i < this.settings.daily.length; i++) {
                // and we are looking for our alimony [i] in 'json.daily' and if found, return the value
                if (json.daily[this.settings.daily[i]] != undefined) {
                  // write the name of this element [i] into the content, remove all underscores from the element [i]
                  content += `<b>${this.settings.daily[i].replaceAll('_', ' ')}:</b>
                  <!--  we take the getMiddleValue function, pass the hour value [i], pass current time "current_weather" and an array time json.hourly[time] there                      -->
                  ${this.getMiddleValue(json.daily[this.settings.daily[i]], json.current_weather.time.substr(0, 10), json.daily['time'])}<br>`
                }
              }
            }
            let icon = ''
            for (let i = 0; i < this.images.length; i++) {
              for (let j = 0; j < this.images[i].code.length; j++) {
                if (this.images[i].code[j] == json.current_weather.weathercode) {
                  icon = this.images[i].path
                }
              }
            }
            // if the icon is not found then it will put it empty
            if (icon == '') {
              icon = require('leaflet/dist/images/marker-icon-2x.png')
            }
            this.coord = {
              id: 1,
              lat: coord.lat.toFixed(4),
              long: coord.lng.toFixed(4),
              icon: icon,
              iconSize: [50, 50],
              content: content
            }

            // we assign content in order to overwrite the contents of the popup

            // this.coord.content = content   ===========  bolwe ne nuzna, udolajem
          })
    },
    // function map click, an object (item) is passed to the click, it has a latitude and longitude in itself
    mapClick(item) {
      // a request comes from api object (item)
      this.requestToApi(item.latlng)
      // set the coordinate point to the center with the current coordinates getCoord()
      this.center = this.getCoord()
      // after the click I make a delay, I turn to the popup and call it
      setTimeout(() => {
        this.$refs.marker.mapObject.openPopup()
      }, 100)
    },
    //a function that returns an object imported from leaflet (latLng)
    // with the current latitude and longitude  this.coord
    getCoord() {
      return latLng(this.coord.lat, this.coord.long)
    },

    // building an API call link
    constructWeatherLink() {
      // create variables
      let url = 'https://api.open-meteo.com/v1/forecast?'
      // we take the link and equate the current coordinates to it
      url = url + `latitude=${this.coord.lat}&longitude=${this.coord.long}`
      // check if there is at least one element in the array
      if (this.settings.hourly != 0) {
        // write the initial text
        let hourly = '&hourly='
        // then add the elements, run a loop over the entire array
        for (let i = 0; i < this.settings.hourly.length; i++)
            // if last element array
          if (this.settings.hourly.length - 1 == i) {
            // take i and write it to the link setting
            hourly += this.settings.hourly [i]
          }
          //otherwise take the element and add a comma
          else {
            hourly += this.settings.hourly [i] + ','
          }
        // write the hourly setting to the link
        url += hourly
      }
      // check if there is at least one element in the array
      if (this.settings.daily != 0) {
        //  write initial text
        let daily = '&daily='
        // then add the elements, run a loop over the entire array
        for (let i = 0; i < this.settings.daily.length; i++)
            // if last element array
          if (this.settings.daily.length - 1 == i) {
            // take i and write it to the link settings
            daily += this.settings.daily [i]
          }
          // otherwise take the element and add a coma
          else {
            daily += this.settings.daily [i] + ','
          }
        // write the hourly setting to the link
        url += daily
      }

      // specify links default settings
      // if the (setting) object and the (temperature) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.temperature != undefined
      ) {
        // we attach in the url the (&temperature_unit=) unit value taken from the object setting variable (temperature)
        url += `&temperature_unit=${this.settings.temperature}`
      }
      // if the (setting) object and the (wind) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.wind != undefined) {
        // we attach in the url the  (&windspeed_unit=) value taken from the object setting variable (wind)
        url += `&windspeed_unit=${this.settings.wind}`
      }
     // if the (setting) object and the (precipitation) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.precipitation != undefined) {
        // we attach in the url the  (&precipitation_unit=) value taken from the object setting variable (wind)
        url += `&precipitation_unit=${this.settings.precipitation}`
      }
      // if the (setting) object and the (timeformat) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.timeformat != undefined) {
        // we attach in the url the  (&timeformat=) value taken from the object setting variable (wind)
        url += `&timeformat=${this.settings.timeformat}`
      }
      // if the (setting) object and the (timezone) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.timezone != undefined) {
        // we attach in the url the  (&timezone=) value taken from the object setting variable (wind)
        url += `&timezone=${this.settings.timezone}`
      }
      // if the (setting) object and the (past) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.past != undefined) {
        // we attach in the url the  (&past_days=) value taken from the object setting variable (wind)
        url += `&past_days=${this.settings.past}`
      }
      // if the (setting) object and the (current) variable are not equal to undefined,
      // then paste the network request link setting
      if (this.settings.current != undefined) {
        // we attach in the url the  (&current_weather=) value taken from the object setting variable (wind)
        url += `&current_weather=${this.settings.current}`
      }
// equate the WeatherLink variable to the url of this "constructWeatherLink()" function and get the input in JSON
      this.weatherLink = url
// and return this value
      return this.weatherLink
    },

    // the function calculates averages, accepts an array of data, current time, and an array of time
    getMiddleValue(array, time, timeArray) {
      let nums = []
      // start iterating through the array with array ( time-array )
      for (let i = 0; i < array.length; i++) {
        // we run through the time array at the same time (timeArray, array ), since they are bound with the same number of values.
        // Checking the current time
        if (timeArray [i] === time) {
          // as soon as the current time is found, in the array of numbers ( let nums = [] ) we push the value
          nums.push(array[i])
        }
      }
      // if array is equal to string
      if (typeof (nums[0]) == 'string') {
        // then we return a specific letter from this string
        return nums[0].substr(11, 5)
      }
      // if it fails the if test, otherwise return the average value of the array, add each element in the array and divide by its length
      return nums.reduce((a, b) => (a + b)) / nums.length
    },

    // function current coordinates
    getLocationGPS() {
         //  The object calls the Geolocation.getCurrentPosition() method to get the device's current location.
         // A callback function that takes one argument ( Position ) as its only input parameter.
         //    we turn to the navigator class, extract the geolocation from it,
         //  and pass it the getCurrentPosition function with an object with one position input parameter
      navigator.geolocation.getCurrentPosition((position) => {
        // write to a variable (position.coords.latitude, position.coords.longitude)
        setLatLong(position.coords.latitude, position.coords.longitude)
      })
      // created a separate function.
        // create a variable, create an unnamed function with 2 arguments (lat. long)
      let setLatLong = (lat, long) => {
        //  calls function mapClick gets item object (latlng) with function imported from leaflet object (latLng ) with variables (lat, long )
        this.mapClick({latlng: latLng(lat, long)})
      }
    },
    // selectHourly function selecting or deselecting values from settings and updates a coordinate
    // incoming value
    selectHourly(value) {
      // it runs through the hourly settings array
      for (let i = 0; i < this.hourly.length; i++) {
        // takes the value from there
        if (this.settings.hourly[i] == value) {
          // if the value is found then deletes it from there
          return this.settings.hourly.splice(i, 1)
        }
      }
      // if the value was not found, then it adds it
      this.settings.hourly.push(value)
      // the mapClick function is called which increases the current coordinates
      this.mapClick({latlng: this.getCoord(this.coord.lat, this.coord.long)})
    },
    // selectDaily function selecting or deselecting values from settings and updates a coordinate
    // incoming value
    selectDaily(value) {
      // it runs through the hourly settings array
      for (let i = 0; i < this.settings.daily.length; i++) {
        // takes the value from there
        if (this.settings.daily[i] == value) {
          // if the value is found then deletes it from there
          return this.settings.daily.splice(i, 1)
        }
      }
      // if the value was not found, then it adds it
      this.settings.daily.push(value)
      // the mapClick function is called which increases the current coordinates
      this.mapClick({latlng: this.getCoord(this.coord.lat, this.coord.long)})
    }

  },
}


</script>


<style scoped>

.map {
  margin-top: 50px;

}


.small_text {
  text-align: center;
  padding-top: 0.5rem;
  padding-bottom: 1rem
}

.coordinate {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

#latitude,
#longitude {
  padding: 12px 24px;
  min-width: 272px;
  margin-right: 17px;
  background: #F9F9F9;
  border: none;
  border-radius: 8px;
  color: rgba(63, 65, 63, 0.93);
  font-family: inherit;
  font-size: inherit;
  cursor: pointer;
  transition: 0.6s;
  margin-top: 24px;
}

.settings_containers {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  margin-left: 10%;
}

.geo-icon {
  margin-top: 1.5rem;
}

.hourly_container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}

.daily_container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

@media screen and (max-width: 905px) {
  .hourly_container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }

  .daily_container {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
}

@media screen and (max-width: 990px) {
  .hourly_container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }
}

@media screen and (max-width: 770px) {
  .hourly_container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    margin-left: 10%;
  }

  .daily_container {
    margin-left: 10%;
  }

  h3 {
    text-align: center;
  }

  .coordinate {
    display: grid;
    grid-template-columns: 1fr;
  }

  .geo-icon {
    display: flex;
    justify-content: center;
  }

  .settings_containers {
    display: grid;
    grid-template-columns: 1fr;
    margin-left: 15%;
  }
}

@media screen and (max-width: 526px) {
  .hourly_container {
    display: grid;
    grid-template-columns: 1fr;
    padding-left: 15%;
  }

  .daily_container {
    display: grid;
    grid-template-columns: 1fr;
    padding-left: 15%;
  }

  h3 {
    text-align: center;
  }
}

.icon {
  vertical-align: bottom;
  background: #F9F9F9;
  cursor: pointer;
  margin-left: 2rem;
}

.icon:hover {
  background-color: rgb(199, 199, 199);

}

.card {
  margin-top: 3rem;
}

.current-checkbox {
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;

}

.form-check-label {
  margin-left: 1rem;
}

.col-3 {
  margin-top: 1rem;

}

select {
  width: 300%;
  height: 35px;
  background-color: white;
  border: 1px solid gainsboro;
  border-radius: 8px;
}

h5 {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: inherit;
  height: 3rem;
  display: flex;
  align-items: center;
  justify-content: center
}

h3 {
  font-weight: 600;
  margin-top: 2.5rem;
  margin-bottom: 2rem;
}

.weatherLink {
  width: 100%;
  height: 40px;
  background-color: #e9ecef;
  border: none;
  border-radius: 4px;
  opacity: 1;
}

.label-margin {
  margin-left: 7px;
}

</style>
