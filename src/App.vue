<template>
  <Results :items="items" />
  <!-- <div>{{ search }}</div> -->
</template>

<script>
import Results from "./components/Results.vue";

export default {
  name: "App",
  data() {
    return {
      items: [
        { title: "Apple", rating: "2", year: "2011" },
        { title: "Banana", rating: "3", year: "2011" },
        { title: "Pear", rating: "4", year: "2011" },
        { title: "Apple2", rating: "2", year: "2011" },
        { title: "Banana2", rating: "3", year: "2011" },
        { title: "Pear2", rating: "4", year: "2011" },
        { title: "Apple3", rating: "2", year: "2011" },
        { title: "Banana3", rating: "3", year: "2011" },
        { title: "Pear3", rating: "4", year: "2011" },
        { title: "Apple4", rating: "2", year: "2011" },
        { title: "Banana4", rating: "3", year: "2011" },
        { title: "Pear4", rating: "4", year: "2011" },
      ],
    };
  },
  components: {
    Results,
  },
  methods: {
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
