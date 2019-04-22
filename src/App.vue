<template>
  <div id="app">
    <beer-selector></beer-selector>
    <!-- <ul>
      <li v-for="(beer, index) in allBeers" :key="index">{{beer.name}}</li>
    </ul> -->
    <beer-list :beers="allBeers"></beer-list>
    <beer-details></beer-details>
  </div>
</template>

<script>
import BeerSelector from "./components/BeerSelector.vue";
import BeerDetails from "./components/BeerDetails.vue";
import BeerList from "./components/BeerList.vue"

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
    "beer-selector": BeerSelector,
    "beer-details": BeerDetails,
    "beer-list": BeerList
  }
}
</script>

<style>

</style>
