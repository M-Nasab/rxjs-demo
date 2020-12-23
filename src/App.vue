<template>
  <div id="app">
    <h1 style="text-align: center;">
      RxJS Demo
    </h1>

    <div class="data-container">
      <div class="inputs">
        <label>
          <span>Country:</span>
          <select>
            <option v-for="country in countries" :key="country.code" :label="country.name" />
          </select>
        </label>

        <label>
          <span>Cities:</span>
          <select>
            <option v-for="city in cities" :key="city.name" :label="city.name" />
          </select>
        </label>

      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      countries: [],
      cities: [],

    };
  },
  mounted() {
  },
  methods: {
    getCountries() {
      return fetch('https://restcountries.eu/rest/v2/all').then((result) => result.json());
    },

    getCities(countryCode) {
      const cities = {
        IR: [
          {
            name: 'Tehran',
          },
          {
            name: 'Mashhad',
          },
          {
            name: 'Qom',
          },
          {
            name: 'Shiraz',
          },
          {
            name: 'Abadan',
          },
        ],

        FR: [
          {
            name: 'Paris',
          },
          {
            name: 'Marseille',
          },
          {
            name: 'Strasbourge',
          },
        ],

        DE: [
          {
            name: 'Berlin',
          },
          {
            name: 'Munich',
          },
          {
            name: 'Hamburge',
          },
        ],
      };

      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(cities[countryCode]);
        }, 2000);
      });
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

.inputs {
  display: flex;

  label {
    font-size: 40px;
    display: block;

    &:not(:last-child) {
      margin-inline-end: 20px;
    }

    span {
      margin-inline-end: 10px;
    }

    select {
      font-size: 40px;
      min-width: 100px;
    }
  }
}
</style>
