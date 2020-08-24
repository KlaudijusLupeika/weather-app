<template>
  <div id="app">
    <div class="search-box">
          <input type="text" 
          class="location"
          id="input"
          placeholder="Click here to enter a destination.." 
          @keyup.enter="fetchWeather"
          v-model="query"
          >
    </div>
    <div 
    class="weather-container"
    :class="typeof weather.main != 'undefined' &&weather.main.temp < 20 ? 'cold' : ''"
    >
        
      <div class="info-wrapper"
      v-if="typeof weather.main !='undefined'">
        <div class="location-container">
          <div class="location-box"> 
            
            {{ weather.name }}, {{ weather.sys.country }}

          </div>
        </div>
        <div class="timezone-box">
          
        It is {{dateBuilder()}} local time.

        </div>
        <div class="weather-details-container">
          <div class="weather-icon">
            <img v-bind:src="'http://openweathermap.org/img/wn/' + this.weather.weather[0].icon + '@2x.png' "  />
          </div>
          <div class="weather-description"> 

            {{ weather.weather[0].main }}
          
            </div>
        </div>
        <div class="weather-temp-box"> 
          <div class="weather-temp">
            {{ ~~weather.main.temp }}°</div>
          <div class="minmax-box">
            <div class="weather-max">{{~~weather.main.temp_max}}°C</div>
            <hr />
            <div class="weather-min">{{~~weather.main.temp_min}}°C</div>
          </div>
        </div>

        <div class="weather-widgets-box">
          <div class="weather-widget">
            <div class="widget-icon">
              <i class="fas fa-thermometer-three-quarters"></i>
            </div>
            <div class="widget-text">
              Feels like
            </div>
            <div class="widget-value">
              {{ ~~weather.main.feels_like }}°
            </div>
          </div>
          <div class="weather-widget">
            <div class="widget-icon">
              <i class="fas fa-wind"></i>
            </div>
            <div class="widget-text">
              Wind speed
            </div>
            <div class="widget-value">
              {{weather.wind.speed}} m/s
            </div>
          </div>
          <div class="weather-widget">
            <div class="widget-icon">
              <i class="fas fa-tint"></i>
            </div>
            <div class="widget-text">
              Humidity
            </div>
            <div class="widget-value">
              {{weather.main.humidity}}%
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {

  name: 'App',
  data () {
    return {
      api_key: '3c56fc758cc4085c0a9f6ab76b768ec8',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      x: '',
      weather: {},
    }
  },
  methods: {
    fetchWeather(){
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then( response => {
          return response.json();
        }).then(this.setResults);
      
      this.query = '';
      document.getElementById('input').blur();
      },
    setResults (results){
      this.weather = results;
      console.log(results);
    },
    dateBuilder() {
      
      let timestamp = new Date().getTime();
      let offset = new Date().getTimezoneOffset();

      let newTimestamp = timestamp + this.weather.timezone*1000 + offset*60000;

      let newdate = new Date(newTimestamp);

      return newdate.toLocaleString('en-US', { hour: 'numeric', minute: 'numeric', hour12: true});

    },

  }
}
</script>

<style>

  * {
  font-family: 'HelveticaNeue-UltraLight', 'Helvetica Neue UltraLight', 'Helvetica Neue', Arial, Helvetica, sans-serif;
  font-weight: 100;
  letter-spacing: 1px;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    color: white;

  }
  body{
    background-image: url('./assets/bg.jpg');
    background-position: bottom;
    background-size: initial;
    background-repeat: no-repeat;
    height: 100vh;
    width: 100vw;
    display: flex;
    align-items: center;
    justify-content: center;

  }
  .weather-container{
    background-image: linear-gradient(#841062, #fa4f39 ) ;
    width: 100%;
    height: 100%;
    box-shadow: 0 0 10px #000;
    transition: 1s;

  }
  .location-container{
    text-align: center;
    padding: 0;
    display: inline-block;
    width: 100%;

  }
  .location-box{
    font-size: 4rem;
    display: block;
    margin-top: 10px;

  }
  .timezone-box{
    font-size: 1rem;
    text-align: center;

  }
  .weather-details-container{
    font-size: 2rem;
  }
  .weather-icon, .weather-description{
    display: inline-block;
  }
  .weather-icon{
    margin: 0 1rem 0 2rem;
  }
  .weather-temp{
    font-size: 10rem;
    margin: 0 0 0 1rem;
    display: inline-block;
  }
  .minmax-box{
    display: inline-block;
    font-size: 1.5rem;
    padding: 0 0 0 2rem;
  }
  .minmax-box hr{
    opacity: 0.6;
    margin: 0 auto;
    width: 80%;
  }
  .weather-max, .weather-min{
    margin: 0.5rem;
    opacity: 0.8;
  }
  .weather-widgets-box{
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .weather-widget{
    text-align: center;
    display: flex;
    background-color: rgba(255, 255, 255, 0.3);
    width: 8rem;
    height: 8rem;
    margin: 3rem 5px;
    border: 2px currentColor;
    border-radius: 3px;
    box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.2);
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
  .widget-icon{
    font-size: 3rem;
    display: block;

  }
  .widget-text{
    margin: 5px 0;
  }
  .widget-value{
    font-size: 2rem;
  }
  input{
    color: black;
    width: 100%;
    font-size: 20px;
    background: none;
    border: none;
    outline: none;
    text-align: center;
    line-height: 40px;
    transition: 1s;
  }
  input:focus{
    background-color: white;
    opacity: 0.7;
  }
  input::placeholder{
    color: white;
    font-size: 10px;
  }
  .cold{
    background-image: linear-gradient(#52a4db, #71b9e1 ) ;
  }



@media only screen and (max-width: 425px) {
    
  .weather-container{
    height: 100%;
    width: 100vw;
  }
  .weather-temp{
    font-size: 40vw;

  }
  .weather-min, .weather-max{
    font-size: 5vw;

  }
  .minmax-box{
    margin-left: 3vw;
    padding: 0;
  }
  .widget-icon{
    font-size: 10vw;
  }
  .widget-value{
    font-size: 5vw;
    color: white;
    font-weight: 300;
  }
  .weather-widget{
    margin-bottom: 10px;
  }
}

</style>