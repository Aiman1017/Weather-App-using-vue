<template>
  <div id="app" class='container'>
    <h1>Weather App Using Vue</h1>

    <div>
      <input type='text' placeholder="Enter A place" v-model='searchTerm'>
      <button v-on:click='buttonPressed' class='btn btn-primary'>Search Place</button>
    </div>

    <div id="weather" v-if="weathers.length != 0">
      <h2>Weather Info</h2>
      <h3>{{city}}</h3>

      <div id='weather-list'>
        <table class='table table-stripe table-bordered' v-on:click="selectWeather(weather)">
          <tr>
            <th>Date Time</th>
            <th>Weather</th>
            <th>Temperature</th>
            <th>Icon</th>
          </tr>

          <tr v-for='weather in weathers' :key="weather.dt">
            <td>{{weather.dt}}</td>
            <td>{{weather.weather[0].main}}</td>
            <td>{{(weather.temp.day - 273.15).toFixed(2)}}C</td>
            <td><img v-bind:src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}03d@2x.png`"></td>
          </tr>
        </table>

      </div>
    </div>

    <div id='weather-info' v-if='selectedWeather'>
      <h3>{{selectedWeather.dt}}</h3>
      <img v-bind:src="`https://openweathermap.org/img/wn/${selectedWeather.weather[0].icon}@2x.png`">
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods:{
    buttonPressed: function(){
      fetch('https://api.openweathermap.org/data/2.5/forecast/daily?q='+this.searchTerm+'&appid=9fd7a449d055dba26a982a3220f32aa2')
      .then(response => response.json())
      .then(data => {
        console.log('Success:', data);
        this.weathers = data["list"]
        this.city = data.city.name
      });
    },

    selectWeather: function(weather){
      this.selectedWeather = weather
    }
  },

  data(){
    return{
      searchTerm: '',
      city: null,
      selectedWeather: null,
      weathers:[]
    }
  }
}
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
</style>
