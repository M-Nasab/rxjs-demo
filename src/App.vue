<template>
  <div id="app">
    <h1 style="text-align: center;">
      RxJS Demo
    </h1>

    <div class="data-container">
      <div v-if="position" class="position">
        {{ `[${position.x}, ${position.y}]` }}
      </div>
    </div>

  </div>
</template>

<script>
import { fromEvent } from 'rxjs';
import { debounceTime } from 'rxjs/operators';

export default {
  data() {
    return {
      position: null,
    };
  },
  mounted() {
    const observer = fromEvent(document, 'mousemove').pipe(
      debounceTime(1000),
    );

    observer.subscribe((event) => {
      this.position = event;
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

.time, .position {
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
