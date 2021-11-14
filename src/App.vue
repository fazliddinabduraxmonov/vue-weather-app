<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp >16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="showWeather">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>


<script>
  export default {
    name: 'app',
    data() {
      return {
        api_key: 'cabcd1b8d633dedc59467775ad7359e6',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
    },
    methods: {
      showWeather(e){
        if (e.key == "Enter") {
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults);
        }
      },
      setResults(results){
        this.weather = results
      },
      dateBuilder(){
        let d = new Date()
        let months = ["January","February","March","April","May","June","July","August","September","Octpber","November","December"]
        let days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]

        let day = days[d.getDay()]
        let date = d.getDate()
        let month = months[d.getMonth()]
        let year = d.getFullYear()

        return `${day} ${date} ${month} ${year}`
      }
    },
  }

</script>


<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/cold.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}
#app.warm{
  background-image: url('./assets/warm.jpg');
}
main{
  display: grid;
  grid-template-columns: 50% 50%;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.4),rgba(0,0,0,0.7));
}
.search-box{
  width: 90%;
}
.search-box .search-bar{
  margin-top: 40vh;
  display: 100%;
  width: 100%;
  padding: 15px;
  color: rgba(0, 0, 0, 0.7);
  font-size: 20px;
  border: none;
  background: none;
  outline: none;
  appearance: none;
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 0px 16px;
  box-shadow: 0 0 8px rgba(0,0,0,0.25);
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0 0 16px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0;
}
.location-box .location{
  margin-top: 15vh;
  color: #ffff;
  font-size: 32px;
  text-align: center;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date{
  color: #ffff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #ffff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather{
  color: #ffff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

</style>
