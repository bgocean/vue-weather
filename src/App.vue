<template>
  <div class="wrapper">
    <h1>Погода сейчас</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <!-- <button v-show="city != ''">Получить погоду</button> -->
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p class="show">{{ showTemp }}</p>
      <p class="show">{{ showFeelsLike }}</p>
      <p class="show">{{ showMinTemp }}</p>
      <p class="show">{{ showMaxTemp }}</p>
      <p class="show">{{ showWind }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 30px;
  padding: 20px;
  background-color: #1f0f24;
  text-align: center;
  color: #fff;

  & h1 {
    margin: 50px 0 20px;
    font-family: sans-serif;
  }

  & p {
    font-family: sans-serif;
  }

  & input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid#110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 15px 8px;
    outline: none;
  }

  & button {
    background-color: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background-color: #746027;
  cursor: not-allowed;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.show {
  margin: 30px 0;
}
</style>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return this.city;
    },
    showTemp() {
      return "Температура: " + this.info.main.temp;
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max;
    },
    showWind() {
      return "Ветер: " + this.info.wind.speed + " м/с";
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно ввести более одного символа :)";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b7e695a0717aa258ea55112f629ca8b2`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>
