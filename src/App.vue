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
import { from } from 'rxjs';

export default {
  data() {
    return {
      list: [],
    };
  },
  mounted() {
    const observer = from([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

    observer.subscribe((data) => {
      this.list.push(data);
    });
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
</style>
