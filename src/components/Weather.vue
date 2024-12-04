<template>
<div class="container p-0">
    <div class="d-flex">
        <div class="card main-div w-100">
            <div class="p-3">
                <h2 class="mb-1 day">Today</h2>
                <p class="text-light date mb-0">{{ date }}</p>
                <small class="text-light date mb-0">{{ time }}</small>
                <h2 class="place"><i class="fa fa-location">{{ name }}<small>{{ spacew }}</small><small>{{ country }}</small></i></h2>
                <div class="temp">
                    <h1 class="weather-temp">{{ temperature }}&deg;</h1>
                    <h2 class="text-light">{{ description }} <img :src="iconUrl"></h2>
                </div>
            </div>
        </div>
        <div class="card card-2 w-100">
        <table class="m-4">
            <tbody>
                <tr>
                    <th>Sea Level</th>
                    <td v-if="sea_level > 0">{{ sea_level }}</td>
                    <td v-else>Null</td>
                </tr>
                <tr>
                    <th>Humidity</th>
                    <td>{{ humidity }}</td>
                </tr>
                <tr>
                    <th>Wind</th>
                    <td>{{ wind }}</td>
                </tr>
            </tbody>
        </table>
        <DaysWeather :cityname="cityname"></DaysWeather>
        <div id="div_Form" class="d-flex m-3 justify-content-center">
            <form action="">
                <input text="button" value="Change Location" @click="changeLocation" class="btn change-btn btn-primary">
            </form>
        </div>
    </div>
    </div>

</div>
    </template>
    
    <script>
    import axios from 'axios';
      import DaysWeather from './DaysWeather.vue';

      export default (await import('vue')).defineComponent({
        name: 'myWeather',
  components: {
    DaysWeather,
  },
  props: {
    city: String, 
  },
  data() {
    return {
      cityname: this.city,
      temperature: null,
      description: null,
      iconUrl: null,
      date: null,
      time: null,
      name: null,
      sea_level: null,
      wind: null,
      country: null,
      spacew: null,
      humidity: null,
      monthNames: ["January", "February", "March", "April", "May", "June",
      "July", "August", "September", "October", "November", "December"],
    }
  },
  methods: {
    changeLocation() {
        window.location.reload();
    }
  },
  async created() {
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ab7473c3b013fd30dbe62a52d805391`)
    const weatherData = response.data;
    this.temperature = Math.round(weatherData.main.temp);
    this.description = weatherData.weather[0].description;
    this.name = weatherData.name;
    this.wind = weatherData.wind.speed;
    this.sea_level = weatherData.main.sea_level;
    this.humidity = weatherData.main.humidity;
    this.country = weatherData.sys.country;
    this.spacew = ',  ';
    this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
    const d = new Date();
    this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
    this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
    console.log(weatherData); 
}
      })
    
    </script>
    
    <style>
body {
    background-color:#343d4b  !important; 
}

.weather-temp {
    color: #fff;  
    margin: 0;
    font-weight: 700;
    font-size: 4em;
}

.place {
    color: #fff;
}

h2.mb-1.day{
    color: #fff;  
    font-size: 3rem; 
    font-weight: 400;
}

.main-div {
    border-radius: 20px;
    color: #fff;
    background-image: url("https://images.unsplash.com/photo-1559963110-71b394e7494d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMD");
    background-size: cover;
    background-position: center;
    background-color: rgba(0, 0, 0, 0.5);
    background-repeat: no-repeat;
}

.temp {
    position: absolute;
    bottom: 0;
}

.main-div:hover{
    transform: scale(1.1);
    transition: transform 0.5s ease;
    z-index: 1;
}

.card-2 {
    color: #fff;
    background-color:#212730; 
    border-radius: 20px;  
}

/* 
P {
  padding: 0 25px 0;
} */

.card-details {
    margin-left: 19px;
}

.h1_Left {
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}
    </style>
    