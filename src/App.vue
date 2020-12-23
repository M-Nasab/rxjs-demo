<template>
  <div id="app">
    <h1 style="text-align: center;">
      RxJS Demo
    </h1>

    <div class="data-container">
      <label class="text-input">
        <span>City Name:</span>
        <input type="text" @input="input">
      </label>

      <div v-if="isLoading" class="spinner">
        {{ loadingText }}
      </div>

      <div v-if="weather" class="weather">
        <div>
          Name: {{ weather.name }}
        </div>
        <div>
          Temperature: {{ weather.main.temp }}
        </div>
        <div>
          Pressure: {{ weather.main.pressure }}
        </div>
        <div>
          Humidity: {{ weather.main.humidity }}
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import {
  from,
  interval,
  Subject,
  throwError,
} from 'rxjs';
import {
  catchError,
  debounceTime,
  delay,
  map,
  mergeMap,
  switchMap,
  tap,
} from 'rxjs/operators';

export default {
  data() {
    return {
      weather: null,
      inputSubject: new Subject(),
      isLoading: false,
      loadingText: '',
    };
  },
  mounted() {
    const loadingTextStream = interval(500).pipe(
      map((time) => {
        const texts = [
          'loading',
          'loading .',
          'loading ..',
          'loading ...',
        ];

        const index = time % 4;

        return texts[index];
      }),
    );

    loadingTextStream.subscribe((text) => {
      this.loadingText = text;
    });

    const observer = this.inputSubject.pipe(
      debounceTime(1000),
      tap(() => {
        this.isLoading = true;
      }),
      switchMap((query) => from(this.getWeather(query))),
      delay(5000),
      mergeMap((result) => (result.cod === 200 ? from([result]) : throwError('error'))),
      map((weather) => {
        if (!weather || !weather.main) return weather;

        return {
          ...weather,
          main: {
            ...weather.main,
            temp: Math.round(weather.main.temp - 273.15),
          },
        };
      }),
      tap(() => {
        this.isLoading = false;
      }),
      catchError(() => observer),
    );

    observer.subscribe((weather) => {
      this.weather = weather;
    });
  },
  methods: {
    getWeather(cityName) {
      const apiKey = '5255a59ae99e219a792a718f9684065f';

      const url = `https://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${apiKey}`;

      return fetch(url).then((result) => result.json());
    },

    input(event) {
      this.inputSubject.next(event.target.value);
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

.text-input {
  font-size: 40px;
  margin-bottom: 30px;
  display: block;

  span {
    font-weight: bold;
    margin-inline-end: 20px;
  }

  input {
    font-size: 30px;
    padding: 10px;
  }
}

.spinner {
  font-size: 40px;
  font-weight: bold;
  text-align: center;

  margin-bottom: 20px;
}
</style>
