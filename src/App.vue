<template>
  <div id="app">
    <beer-selector></beer-selector>
    <ul>
      <li v-for="(beer, index) in allBeers" :key="index">{{beer.name}}</li>
    </ul>
  </div>
</template>

<script>
import BeerSelector from "./components/BeerSelector.vue";

export default {

  name: 'app',
  data() {
    return{
      allBeers: []
    }
  },
  mounted(){
    fetch("https://api.punkapi.com/v2/beers")
      .then((res) => res.json())
      .then((data) => this.allBeers = data);
  },
  components: {
    "beer-selector": BeerSelector
  }
}
</script>

<style>

</style>
