<template> 

    <div v-if="hasAPI" id="container">
        
        <!-- left side -->
        <div class="weather-img">

            <div class="weather-temp">
                <img class="weather-state" v-bind:src="imgSource" alt="Current weather"> 
                <h1 class="temp-text">{{temp}} &deg;C</h1>
            </div>

            <p class="weather-description">{{description}}</p>
            <p class="weather-feel-like">Feels like {{feel_like}}&deg;C</p>

            <div class="humidity-wind-container">
                <div class="humidity">
                    <h3>Humidity</h3>
                    <p>{{humidity}}%</p>
                </div>
                <div class="wind">
                    <h3>Wind</h3>
                    <p>{{wind}}km/h</p>
                </div>
            </div>
        </div>

        <!-- Expand weather info -->
        <div class="show-info-weather" @click="changeSize()">
            <button >
                <img class="icon-switching"  src="https://img.icons8.com/external-those-icons-flat-those-icons/24/000000/external-Arrow-arrows-those-icons-flat-those-icons-45.png"/>
            </button>       
        </div>
        
        <!-- right side -->
        <div class="weather-info">
            <h1 class="info-text text-30">{{name}}</h1> 
            <h1 class="info-text"> {{time()}} </h1>     
            <p class="line"> </p> 
            <div class="one-line-cover">
                <h1 class="info-text"> Sun rise {{sun_rise}} </h1>     
                <h1 class="info-text"> Sun set {{sun_down}} </h1>    
            </div>
            <h1 class="info-text">
                Visibility:
                {{visibility}}km
            </h1> 
        </div>
    </div>
</template>

<script>
    
import moment from "moment";
export default {
name: 'WeatherInfo',
props: ['dataAPI'], 
data() {
    return{
        hasAPI: false,
        name: '',
        temp: '',
        visibility: '',
        humidity: '',
        description: '',
        wind: '',
        feel_like: '',
        sun_rise: '',
        sun_down: '',
        size: '333px'
    }
},
watch: {
    dataAPI: function() {
        this.hasAPI = true
        this.handleAPI(this.dataAPI)
    }
},
computed:{
    imgSource() {
        if(this.dataAPI.weather[0].icon){
            return `http://openweathermap.org/img/wn/${this.dataAPI.weather[0].icon}@2x.png`;
        }           

        return "http://openweathermap.org/img/wn/10d@2x.png";
    }

},
methods:{
    handleAPI(data){
        if(data){
            this.name = data.name
            this.temp = Math.round(data.main.temp - 273.15)
            this.visibility = Math.round((data.visibility) / 1000)
            this.humidity = data.main.humidity
            this.description = data.weather[0].description
            this.wind = Math.round(data.wind.speed * 3,6)
            this.feel_like = Math.round(data.main.feels_like - 273.15) 
            this.sun_rise = this.sunTime(data.sys.sunrise) 
            this.sun_down = this.sunTime(data.sys.sunset) 
        } else{
            return "--"
        }
    },

    changeSize(){
        var container = document.querySelector('#container')
        var weatherInfo = document.querySelector('.weather-info')
        var showInfoWeather = document.querySelector('.show-info-weather')
        var containerStyle = container.style.width

        //Expand box weather info
        if(containerStyle == '700px' ) {
            container.style.setProperty('width', '333px')
            weatherInfo.style.setProperty('display', 'none')
            document.querySelector('.icon-switching').src = 'https://img.icons8.com/external-those-icons-flat-those-icons/24/000000/external-Arrow-arrows-those-icons-flat-those-icons-45.png'

            showInfoWeather.style.borderTopRightRadius = '15px'
            showInfoWeather.style.borderBottomRightRadius = '15px'
            showInfoWeather.style.borderTopLeftRadius = '0px'
            showInfoWeather.style.borderBottomLeftRadius = '0px'

        }else{
            container.style.setProperty('width', '700px')
            weatherInfo.style.setProperty('display', 'flex')
            document.querySelector('.icon-switching').src = 'https://img.icons8.com/external-those-icons-flat-those-icons/24/000000/external-Arrow-arrows-those-icons-flat-those-icons-46.png'

            showInfoWeather.style.borderTopRightRadius = '0px'
            showInfoWeather.style.borderBottomRightRadius = '0px'
            showInfoWeather.style.borderTopLeftRadius = '15px'
            showInfoWeather.style.borderBottomLeftRadius = '15px'

        }
    },

    time() {
        const date = moment(new Date());
        return date.locale("en").format("dddd, Do MMMM");
     },

    sunTime(valueAPI) {
        const sunTime = valueAPI
        return moment.unix(sunTime).format("H:mm")
    }
}
}
</script>

<style scoped>
@import '../../src/assets/css/style.css';


#container{
    margin: 0 auto;
    display: flex;
    box-shadow: -1px 4px 28px 0px rgba(0,0,0,0.75);
    border-radius: 15px;
    background: rgba(0, 0, 0, 0.4);
    width: 333px;
}


/* Left side */
.weather-img{
    position: relative;
}

.weather-temp{
    display: flex;
    align-items: center;
    padding: 10px;
}

.weather-state{
    height: 150px;
}

.temp-text{
    font-size: 40px;
    color: var(--white-color);
    margin-right: 20px;
}

.weather-description{
    font-weight: bolder;
    text-transform: uppercase;
    color: var(--white-color);
    font-size: 20px;
}

.weather-feel-like{
    color: var(--white-color);
    font-size: 20px;
    font-weight: bolder;
}


.humidity-wind-container{
    font-size: 20px;
    display: flex;
    justify-content: space-around;
    font-weight: bolder;
    color: var(--white-color);
}

.show-info-weather{
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
}

.show-info-weather:hover{
    background-color: rgba(226, 220, 223, 0.8);
    border-top-right-radius: 15px;
    border-bottom-right-radius: 15px;
    opacity: 30%;
}

.icon-switching{
    font-size: 20px;
    fill: aqua;
}


/* right side */
.weather-info{
    display: none;
    flex: 1;
    flex-direction: column;
    justify-content: space-evenly;
    border-top-right-radius: 15px;
    border-bottom-right-radius: 15px;
}

.info-text{
    color: var(--white-color);
    text-transform: uppercase;
    font-weight: 300;
}

.text-30{
    font-size: 50px;
    font-weight: bolder;
}

.line {
    width: 80%;
    border: 1px solid black;
    margin-left: 10%;
    margin-bottom: 20px;
}

.one-line-cover{
    display: flex;
}

</style>