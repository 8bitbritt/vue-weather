<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 70 ? 'warm' : ''">
   
    <main>
  
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="getWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-container">
          <div class="location">Weather In {{ weather.name }}, {{ weather.sys.country }}</div>
        </div>

        <div class="weather-container">
          <div class="temp weather-condition">
            {{ Math.round(weather.main.temp) }}°f <span class="thin">|</span> <img id="icon" :src="require(`./assets/` + weather.weather[0].icon + `.gif`)"/>
           
            </div>
             <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
 
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '225515981206f0e5875bc0324290e406',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    getWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Lato', sans-serif;
}

#app {
  background: rgb(35,122,181);
background: linear-gradient(180deg, rgba(144,105,98,1) 0%, rgba(39,28,46,1) 100%);
  transition: 1s;
  height: 100vh;
  width: 100%;
  display:flex;
  align-items:center;
  color: #efefef;
}

#app.warm {
  background: #241C2D;
background: linear-gradient(180deg, rgba(167,94,79,1) 0%, rgba(42,57,49,1) 100%);
}

.thin{
  font-weight: 300;
  padding-left: 10px;
  padding-right: 10px;
}

main {
  padding: 25px;
  height: 400px;
  width: 80%;
  max-width:700px;
  margin: 0 auto;
 background: transparent;
 border: 2px solid rgba(255,255,255,.5);
 border-radius: 25px;
}

.search-box {
  width: 100%;
  max-width: 400px;
  margin-bottom: 30px;
  margin-left: auto;
  margin-right: auto;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  border: 2px solid rgba(255,255,255,.5);
  background-color: transparent;
  border-radius: 25px;
}

::placeholder{
  color: #efefef;
}

.search-box input{
  color:#efefef;
}

.search-box .search-bar:focus {
  border: 2px solid #FFF;
}

.location-container .location {
  font-size: 32px;
  font-weight: 700;
  text-align: center;
  letter-spacing: 1px;
  color:rgb(228 210 207);
}

.weather-condition{
  display:flex;
  align-items:center;
  justify-content: center;
}

.weather-container {
  text-align: center;
}

.weather-container .temp {
  padding: 10px 25px;
  font-size: 60px;
  line-height: 1;
  font-weight: 400;
  margin: 30px 0px;
}

.weather{
  font-size: 28px;
  color: rgba(255,255,255,.8);
}
#icon{
  max-width:60px;
}
@media only screen and (max-width: 600px){
  main{
    width: 95%;
    height: 100%;
    border:none;
  }
  .location-container .location {
  font-size: 6vw;
}
.weather-container .temp {
  font-size: 12vw;
}
#icon{
  max-width: 50px;
}
}

</style>
