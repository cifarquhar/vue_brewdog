<template>
  <div id="app">
    <h1>BrewDog Beer Recipe Selector</h1>
    <beer-selector :yeasts="yeasts" :malts="malts" :hops="hops"></beer-selector>
    <beer-list :beers="filteredData"></beer-list>
    <beer-details :beer="currentBeer" :showModal="showModal"></beer-details>
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
      hops: [],
      showModal: false
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
      .then(() => this.filterHops())
      .then(() => this.filterMalts())

      eventBus.$on('beer-selected', (beer) => {
        this.currentBeer = beer;
      })

      eventBus.$on('beers-filtered', (beers) => {
        this.filteredData = beers;
      })

      eventBus.$on('modal-state-change', (value) => {
        this.showModal = value;
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
    },
    filterHops: function(){
      const hopsPerBeer = this.allBeers.map(beer => beer.ingredients.hops)
      const hops = hopsPerBeer.flat();
      this.hops = [... new Set(hops.map(hop => hop.name))]
    },
    filterMalts: function(){
      const maltsPerBeer = this.allBeers.map(beer => beer.ingredients.malt)
      const malts = maltsPerBeer.flat();
      this.malts = [... new Set(malts.map(malt => malt.name))]
    }
  }
}
</script>

<style>

</style>
