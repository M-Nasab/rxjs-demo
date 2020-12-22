<template>
  <div id="app">
    <h1 style="text-align: center;">
      RxJS Demo
    </h1>

    <div class="data-container">
      <ul>
        <li v-for="(data, index) in list" :key="index">
          {{ data }}
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
import { interval } from 'rxjs';
import { filter, map } from 'rxjs/operators';

export default {
  data() {
    return {
      list: [],
    };
  },
  mounted() {
    const observer = interval(1000).pipe(
      filter((data) => data % 5 === 0),
      map((data) => data / 2),
    );

    observer.subscribe((time) => {
      this.list.push(time);
    });
  },
  methods: {
    getWeather(cityName) {
      const apiKey = '5255a59ae99e219a792a718f9684065f';

      const url = `https://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${apiKey}`;

      return fetch(url).then((result) => result.json());
    },
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@400;700&display=swap');

* {
  box-sizing: border-box;
}

html, body {
  font-family: 'Amatic SC', cursive;
  margin: 0;
  padding: 0;
}

h1 {
  font-size: 70px;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
  width: 100%;
  height: 100vh;
  padding: 100px;
  padding-top: 50px;
}

.data-container {
  border: 4px solid black;
  overflow: auto;
  padding: 20px;
}

ul {
  font-size: 40px;
  margin: 0;
}

.time {
  font-size: 80px;
  font-weight: bold;
  text-align: center;
  color: cornflowerblue;
}

.weather {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
  font-size: 50px;
  font-weight: bold;

  & > div {
    padding: 20px;
    border: 1px solid chocolate;
  }

  justify-items: center;
  align-items: center;
}
</style>
