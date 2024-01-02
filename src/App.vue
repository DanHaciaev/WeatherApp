<template>
  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" @keyup.enter="getWeather">
    <button v-if="city != ''" @click="getWeather()">Получить данные</button>
    <button disabled v-else>Введите город</button>
    <p className="error">{{ error }}</p>
    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "'" + this.city + "'"
    },
    showTemp() {
      const temperature = Math.round(this.info.main.temp);
      const sign = temperature >= 0 ? '+' : '-';
      return `Температура: ${sign}${Math.abs(temperature)}°C`;
    },
    showFeelsLike() {
      const feelsLike = Math.round(this.info.main.feels_like);
      const sign = feelsLike >= 0 ? '+' : '-';
      return `Ощущается как: ${sign}${Math.abs(feelsLike)}°C`;
    },
    showMinTemp() {
      const minTemp = Math.floor(this.info.main.temp_min);
      const sign = minTemp >= 0 ? '+' : '-';
      return `Минимальная температура: ${sign}${Math.abs(minTemp)}°C`;
    },
    showMaxTemp() {
      const maxTemp = Math.ceil(this.info.main.temp_max);
      const sign = maxTemp >= 0 ? '+' : '-';
      return `Максимальная температура: ${sign}${Math.abs(maxTemp)}°C`;
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false;
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
        .then(res => (this.info = res.data))
    }
  }
}
</script>

<style scoped >
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgb(5, 168, 229);
  background: linear-gradient(146deg, rgba(5, 168, 229, 1) 0%, rgba(255, 251, 4, 1) 100%);
  text-align: center;
  color: white;
}

.wrapper h1 {
  padding-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid silver;
  color: black;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: white;
}

.wrapper button:disabled {
  background: #706239;
  cursor: not-allowed;
}

.wrapper button {
  background: #e3bc4d;
  color: white;
  border-radius: 10px;
  border: 2px solid silver;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>