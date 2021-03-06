<template>
  <Results />
</template>

<script>
import Results from "./components/Results.vue";

export default {
  name: "App",
  components: {
    Results,
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}
#app {
  max-width: 1200px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 30px;
}

.hidden {
  display: none;
}
</style>
