<template>
  <div id="app">
    <h1 style="text-align: center;">
      RxJS Demo
    </h1>

    <div class="data-container">
      <div class="time">
        {{ time }}
      </div>
    </div>

  </div>
</template>

<script>
import { interval } from 'rxjs';

export default {
  data() {
    return {
      time: 0,
    };
  },
  mounted() {
    const observer = interval(1000);

    observer.subscribe((data) => {
      this.time = data;
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

.time {
  font-size: 80px;
  font-weight: bold;
  text-align: center;
  color: cornflowerblue;
}
</style>
