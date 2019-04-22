<template>
  <div id="app">
    <beer-selector></beer-selector>
    <beer-list :beers="allBeers"></beer-list>
    <beer-details :beer="currentBeer"></beer-details>
  </div>
</template>

<script>
import BeerSelector from "./components/BeerSelector.vue";
import BeerDetails from "./components/BeerDetails.vue";
import BeerList from "./components/BeerList.vue";
import { eventBus } from "./main.js";

export default {

  name: 'app',
  data() {
    return{
      allBeers: [],
      currentBeer: null
    }
  },
  mounted(){
      eventBus.$on('beer-selected', (beer) => {
        this.currentBeer = beer;
      })

      eventBus.$on('beers-filtered', (beers) => {
        this.allBeers = beers;
      })
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
