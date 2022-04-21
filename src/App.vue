<template>
  <div id="app">
      <button class="btn" v-if="!visible" @click="getWeather">Locate me</button>
      <div class="card" v-if="visible">
        <p>City: {{weather.city}}</p>
        <p>Country: {{weather.country}}</p>
        <p>{{weather.desc}}</p>
        <p>Temperature: {{weather.temp}} &#176;C</p>
        <p>Min Temperature: {{weather.lowTemp}} &#176;C</p>
        <p>Max Temperature: {{weather.highTemp}} &#176;C</p>
        <p> Feels Like: {{weather.feelsLike}} &#176;C</p>
        <p> Humidity: {{weather.humidity}} &#37;</p>
    </div>
  </div>
</template>

<script>

export default {
    data() {
        return {
            visible: false,
            weather:{
                city: "",
                country:"",
                desc:"",
                temp: "",
                lowTemp:"",
                highTemp:"",
                feelsLike:"",
                humidity:""
            }
        }
    },
    methods: {
        getWeather: function (){
            if (navigator.geolocation){
            navigator.geolocation.getCurrentPosition(async position => {
            const baseURL = `https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=c6b6c8307d72d5c708f21b3831d3b5f4&units=metric`;
            const response = await fetch(baseURL);
            const data = await response.json();
            this.weather.city = data.name;
            this.weather.country = data.sys.country;
            this.weather.desc = data.weather[0].description;
            this.weather.temp = Math.round(data.main.temp);
            this.weather.lowTemp = Math.round(data.main.temp_min);
            this.weather.highTemp = Math.round(data.main.temp_max);
            this.weather.feelsLike = Math.round(data.main.feels_like);
            this.weather.humidity = data.main.humidity;
            this.visible = true;
            })}
        }}}
</script>

<style>
html{
    overflow: hidden;
}
#app{
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    color: white;
    overflow: hidden;
}
.btn{
    box-shadow:inset 0px 1px 0px 0px #97c4fe;
	background:linear-gradient(to bottom, #3d94f6 5%, #1e62d0 100%);
	background-color:#3d94f6;
	border-radius:6px;
	border:1px solid #337fed;
	display:inline-block;
	cursor:pointer;
	color:#ffffff;
	font-family:Arial;
	font-size:15px;
	font-weight:bold;
	padding:10px 24px;
	text-decoration:none;
	text-shadow:0px 1px 0px #1570cd;
    text-transform: uppercase;
}
.btn:hover {
	background:linear-gradient(to bottom, #1e62d0 5%, #3d94f6 100%);
	background-color:#1e62d0;
}
.btn:active {
	position:relative;
	top:1px;
}
.card{
    width: 30vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    background: linear-gradient(4deg, rgba(2,0,36,1) 0%, rgba(65,66,147,0.9920343137254902) 43%, rgba(0,212,255,1) 100%);
    border-radius: 50px;
}
p:nth-child(3){
    text-transform: uppercase;
}
</style>
