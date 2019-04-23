<template>
  <div id="app">
    <beer-selector></beer-selector>
    <beer-list :beers="filteredData"></beer-list>
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
      currentBeer: null,
      filteredData: [],
      yeasts: [],
      malts: [],
      hops: []
    }
  },
  mounted(){
    const fetchPage1 = fetch("https://api.punkapi.com/v2/beers?page=1&per_page=80");
    const fetchPage2 = fetch("https://api.punkapi.com/v2/beers?page=2&per_page=80");
    const fetchPage3 = fetch("https://api.punkapi.com/v2/beers?page=3&per_page=80");
    const fetchPage4 = fetch("https://api.punkapi.com/v2/beers?page=4&per_page=80");
    const fetchPage5 = fetch("https://api.punkapi.com/v2/beers?page=5&per_page=80");

    Promise.all([fetchPage1, fetchPage2, fetchPage3, fetchPage4, fetchPage5])
      .then((responses) => Promise.all(responses.map(res => res.json())))
      .then(result => this.allBeers = result.flat())
      .then(() => this.filterYeast())

      eventBus.$on('beer-selected', (beer) => {
        this.currentBeer = beer;
      })

      eventBus.$on('beers-filtered', (beers) => {
        this.filteredData = beers;
      })
  },
  components: {
    "beer-selector": BeerSelector,
    "beer-details": BeerDetails,
    "beer-list": BeerList
  },
  methods: {
    filterYeast: function(){
      this.yeasts = [... new Set(this.allBeers.map((beer) => {
        return beer.ingredients.yeast
      }))]
    }
  }
}
</script>

<style>

</style>
