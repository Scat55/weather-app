<script setup>
import {onMounted, ref} from 'vue';
import axios from 'axios';

const dataWeather = ref({});
const icon = ref('')
const temp = ref(0)
const descr = ref('')
const date = new Date()
const options = { weekday: 'long' };
const dayOfWeek = date.toLocaleString('ru', options);
const resultName = dayOfWeek.charAt(0).toUpperCase() + dayOfWeek.slice(1)

const dayNow = date.getDate()
const monthNow = date.getMonth()
const yearNow = date.getFullYear()



const getAPI = async () => {
  try {
    await axios('https://api.openweathermap.org/data/2.5/weather?lat=52.609940&lon=39.599222&lang=ru&appid=f5db913198adccbc7f7886b71cf11121').then((res) => {
      dataWeather.value = res.data
      icon.value = res.data.weather[0].icon
      temp.value = res.data.main.temp
      descr.value = res.data.weather[0].description
      console.log(res.data)
    });
  } catch (err) {
    console.log(err);
  }
};

// const getAPIFourDays = async () => {
//   try {
//     await axios('https://pro.openweathermap.org/data/2.5/forecast/hourly?lat=52.609940&lon=39.599222&lang=ru&appid=f5db913198adccbc7f7886b71cf11121')
//     .then((res) => {
//         console.log(res)
//       });
//   } catch(err) {
//     console.log(err)
//   }
// }

onMounted(() => {
  getAPI();
  // getAPIFourDays()
})


</script>

<template>
  <div class="weather">

    <div class="weather__window">
      <h2 class="weather__title"> {{ dataWeather.name }}</h2>
      <p class="weather__subtitle">{{resultName}} {{dayNow}}.{{monthNow}}.{{yearNow}}</p>

      <div class="weather__degrees">
        <img
          :src="`https://openweathermap.org/img/wn/${icon}@2x.png`"
          alt="weatherIcon"
          class="weather__degrees-img"
        />
        <p class="weather__degrees-title"> {{ (temp - 273.15).toFixed() }} <sup>o</sup>C</p>
      </div>
      <p class="weather__descr">
        {{ descr }}</p>
    </div>

  </div>
</template>

<style scoped lang="scss">
.weather {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;

  &__window {
    width: 548px;
    height: 660px;
    flex-shrink: 0;
    border-radius: 30px;
    background: linear-gradient(144deg, rgba(255, 255, 255, 0.11) 0%, rgba(255, 255, 255, 0.22) 100%);
    filter: blur(0px);
    backdrop-filter: blur(25px);
    padding: 20px;
  }

  &__title {
    color: #fff;
    text-align: center;
    font-size: 1.5rem;

  }
  &__subtitle {
    color: #fff;
    text-align: center;
    font-size: 0.8rem;
    margin-top: 8px;
  }
  &__degrees {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 49px;
    gap: 20px;



    &-title {
      color: #fff;
      text-align: center;

      font-size: 4rem;
      font-weight: bold;
      align-self: center;
    }

    &-img {
      width: 100px;
      height: 100px;
      align-self: end;
    }
  }

  &__descr {
    color: #373737;
    text-align: center;
    margin-top: 1rem;
    font-size: 1.5rem;
  }
}
</style>
