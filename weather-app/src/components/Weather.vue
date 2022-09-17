<template>
<div class="container">
    
    <div class="content">

        <v-btn
        v-if="show"
            color="red"
            elevation="2"
            @click="closeBox()"    
            >
           Cancel
        </v-btn>

        <v-btn
        v-else
            color="primary"
            elevation="2"
            @click="openBox()"    
            >
           Start
        </v-btn>

        
       <div class="input-city">
        <v-text-field 
            v-if="inputing"
            ref="weatherInput"
            color="primary"
            shaped
            outlined
            v-model="inputText"
            class="custom-label-color"
            label="Input your city"
            placeholder="Enter your city"
            append-icon="X"
            @click:append="clearText()"
            @keyup.enter="getAPI()"
            >
        </v-text-field>
       </div>

        <WeatherInfo :dataAPI="dataAPI" :inputText="inputText" />
    </div>
</div>


</template>

<script>
import axios from 'axios'
import WeatherInfo from './WeatherInfo.vue'
export default {
    name: "Weather",
    components: { WeatherInfo },
    data() {
        return {
            show: false,
            alertBox: 'alertBox',
            inputing: false,
            inputText: '',
            dataAPI: null,
            key: "c4e7b399b485192946342267e9f1dfa9",
            rules: {
                name: [val => (val || '').length > 0 || 'This field is required']
            }
        };
    },
    methods: {
        clearText() {
            this.inputText = ''               
            this.$refs.weatherInput.focus()         
        },

        showToastMessage() {
            const app = document.getElementById('app')
                const toast = document.createElement('div')
                toast.classList.add('toast')
                toast.style.animation = ''
                toast.innerHTML =  `
                    <img class="toast-icon" src="https://img.icons8.com/external-sbts2018-outline-color-sbts2018/58/000000/external-alert-emergencies-set-sbts2018-outline-color-sbts2018.png"/>
                    
                    <div class="toast-message">
                            <h3>Danger</h3>
                            <h5>There's not have any city</h5>
                    </div>
                `
                app.appendChild(toast)

                setTimeout(() => {
                    app.removeChild(toast)
                },3000)
        },

        closeBox(){
            this.inputing = false
            this.show = !this.show
        },

        openBox(){
            this.inputing = true
            this.show = !this.show
        },
        
        getAPI() {
            if(this.inputText == '' ) {
                this.showToastMessage()
                this.$refs.weatherInput.focus();           
            }
            else {
                try {
                    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.inputText}&appid=${this.key}`)
                    .then(response => {
                        this.dataAPI = response.data;
                        // this.$emit("get-API", this.dataAPI)
                        this.inputText = ''               
                    })  
                    .catch(() => {
                        this.showToastMessage()
                        this.inputText = ''               
                    });
                }
                catch (error) {
                   if (error.response) {
                        console.log(err.response.data);
                        console.log(err.response.status);
                        console.log(err.response.headers);
                    } else if (error.request) {
                        console.log(err.request);
                    }
                    else {
                        console.log('Error', err.message);
                    }
                }             
            }     
        }
    }
}
</script>

<style>
.down-arrow{
    fill: var(--white-color);
}

.custom-label-color input{
  color: var(--white-color)!important;
}
.input-city {
  margin-top: 20px;
}
.container{
    margin: 0 auto;
    position: relative;
}

.content{
    margin: 0 auto;
    width: 700px;
}

/* toast  */
.toast{
    background-color: #FF4949;
    position: absolute;
    top: 20px;
    right: 10px;
    border-radius: 15px;
    height: 60px;
    width: 250px;
    display: flex;
    align-items: center;
    animation: slideIn  ease 1.5s, fadeOut linear 1s 3s;
    box-shadow: -1px 2px 10px 0px rgba(0,0,0,0.75);
}

.toast-message{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    color: var(--white-color);
}

.toast-icon{
    height: 30px;
    padding: 0 10px;
}

@keyframes slideIn{
    from {
        right: -40px;       
        opacity: 0;
    }
    to {
        right: 10px;
        opacity: 1;
    }
}

@keyframes fadeOut{
    to{
        opacity: 0;
    }
}

</style>