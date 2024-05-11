<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null,
    };
  },
  computed: {
    showTemp() {
      return 'Температура:' + this.info.temp;
    },
    showFeelsLike() {
      return 'Ощущается как:' + this.info.feels_like;
    },
    showMinTemp() {
      return 'Минимальная температура:' + this.info.temp_min;
    },
    showMaxTemp() {
      return 'Максимальная температура:' + this.info.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 4) {
        this.error = 'Название должно быть более четырех символов!';
        return false;
      }
      this.error = '';
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=c4d00392dd41db0e7a69a2f406e06073&units=metric`
        )
        .then((res) => (this.info = res.data.main));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в городе {{ city == '' ? 'вашем городе' : city }}</p>
    <input type="text" placeholder="Введите название города" v-model="city" />
    <br />
    <button @click="getWeather()">Получить прогноз</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 100%;
  height: 80%;
  padding: 30px;
  border-radius: 50px;
  background: rgb(16, 26, 76);
  text-align: center;
  color: white;
}

.wrapper h1 {
  margin-top: 0;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(16, 26, 76);
  color: aliceblue;
  font-size: 20px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom: 2px solid blue;
}

.wrapper button {
  background: rgba(20, 160, 243, 1);
  color: white;
  width: 180px;
  height: 45px;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  margin-left: 50px;
  font-size: 16px;
  transition: transform 1s linear;
  margin: 15px 0;
}

.wrapper button:hover {
  transform: scale(1.1);
  background: rgb(70, 233, 129);
  background: linear-gradient(
    165deg,
    rgba(70, 233, 129, 1) 0%,
    rgba(20, 160, 243, 1) 100%
  );
  color: white;
}

.error {
  color: red;
}

@media (max-width: 768px) {
  .wrapper {
    width: 80%;
    height: fit-content;
    padding: 20px;
    margin: auto;
  }
}
</style>
