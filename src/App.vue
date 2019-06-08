<template>
  <div id="app">
    <p>
      Show All
      <input type="checkbox" name="vehicle" value="Bike" @change="showAllFunc">
    </p>
    <data-table :list="movieList" :showAll="showAll" :key="showAll"/>
    
  </div>
</template>

<script>
import DataTable from "./components/DataTable.vue";
const movie = require("./assets/movie.json");

export default {
  name: "app",
  data() {
    return {
      movieList: movie,
      showAll: false
    };
  },
  components: {
    DataTable
  },

  methods: {
    showAllFunc() {
      this.showAll = !this.showAll;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
