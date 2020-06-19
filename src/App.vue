<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warn' : ''">
    <main>
      <div id='search_box'>
        <input type="text" class="search_bar" placeholder="Search..." v-model="query" @keypress="fetchWeather"/>
        <!-- {{ query }} -->
      </div>

      <div id='content_box' v-if="typeof weather.main != 'undefined'">
        <div class="location_box">
          <div id='location'> {{ weather.name }}, {{ weather.sys.country }} </div>
          <div id='date'> {{ dateBuilder() }} </div>
        </div>

        <div class='weather_box'>
          <div id='temp'>{{ Math.round(weather.main.temp) }}°C</div>
          <div id='weather'> {{ weather.weather[0].main }} </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default{
  name: 'app',
  data(){
    return{
      api_key: ' :D ',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query : '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e){
      if (e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
  },
  setResults(results){
    this.weather = results;
    },
    dateBuilder (){
      let today = new Date();
      let date = today.toLocaleDateString('locale', {weekday: 'long'})+' '+today.getDate()+' '+today.toLocaleString('default', {month: 'long'})+' '+today.getFullYear();
      let dateTime = date;
      return dateTime;
    }
  }
}

</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app{
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-image: url("../assets/cold_back.jpg");
  background-size: cover;
  background-position: bottom;
  font-family: Arial, sans-serif;
  transition: 0.8s;
}
#app.warn{
  background-image: url("../assets/hot_back.jpg");
}
main{
  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.30), rgba(0,0,0,0.60));
}
#search_box{
  width: 100%;
  margin: 5% auto 0 auto;
  padding: 25px;
}
.search_bar{
  font-size: 20px;
  padding: 10px;
  border: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 10px 0px 10px;
  box-shadow: 0px 0px 8px 0px;
  width: 100%;
  transition: 0.4s;
  outline: none;
}
.search_bar:focus{
  background-color:rgba(255, 255, 255, 0.8) ;
  border: none;
  box-shadow: 0px 0px 16px 0px;
}
#content_box{
  height: auto;
  text-align: center;
  padding: 10px;
  color: white;
}
.location_box{
  margin: 15px;
}
#location{
  font-size: 35px;
  font-weight: 500;
  text-shadow: 1px 4px rgba(0,0,0,0.25);
}
#date{
  font-size: 20px;
  font-style: italic;
  font-weight: 300;
}
.weather_box{
  text-align: center;
}
#temp{
  display: inline-block;
  padding: 10px 30px;
  font-size: 100px;
  border-radius: 4%;

  text-shadow: 1px 5px rgba(0,0,0,0.40);
  background-color: rgba(255,255,255,0.25);
}
#weather{
  margin-top: 5%;
  font-size: 40px;
  text-shadow: 3px 6px rgba(0,0,0,0.30);
  font-style: italic;
}
</style>
