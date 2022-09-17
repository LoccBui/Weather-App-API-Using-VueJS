<template>
  <v-app>
   <div id="app">
      <Header />
      <Weather />  
   </div>
  </v-app>
</template>

<script>
import Header from './components/Header.vue';
import Weather from './components/Weather.vue';

export default {
  name: 'App',

  components: {
    Header,
    Weather
  }
 

};
</script>

<style scoped>
  #app{
    background: url('./assets/img/background-weather.jpg') no-repeat center center fixed;
    background-size: cover;
    text-align: center;
    animation: change 20s infinite none;
  }


  @keyframes change{
    0% {
      background: url('./assets/img/background-weather.jpg') no-repeat center center fixed
    }
    20% {
      background: url('./assets/img/background1.jpg') no-repeat center center fixed
    }
    40% {
      background: url('./assets/img/background2.jpg') no-repeat center center fixed
    }
    60% {
      background: url('./assets/img/background-weather.jpg') no-repeat center center fixed
    }
    80% {
      background: url('./assets/img/background1.jpg') no-repeat center center fixed
    }
    100% {
      background: url('./assets/img/background2.jpg') no-repeat center center fixed
    }
  }

</style>
