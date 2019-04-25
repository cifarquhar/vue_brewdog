<template lang="html">
  <div>
    <button v-on:click="handleClick">Get Random Beer</button>
    <form v-on:submit="handleSubmit">
      <div class="partition">
        <label for="name">Name: </label>
        <input id="name" name="beer_name" type="text">
      </div>
      <div>
        <label for="min-abv">Min. ABV: </label>
        <input id="min-abv" name="abv_gt" type="number" min=0 placeholder=0>
        <label for="max-abv">Max. ABV: </label>
        <input id="max-abv" name="abv_lt" type="number" min=0 placeholder=0>
      </div>
      <div class="partition">
        <label for="min-ibu">Min. IBU: </label>
        <input id="min-ibu" name="ibu_gt" type="number" min=0 placeholder=0>
        <label for="max-ibu">Max. IBU: </label>
        <input id="max-ibu" name="ibu_lt" type="number" min=0 placeholder=0>
      </div>
      <div class="partition">
        <label for="min-ebc">Min. EBC: </label>
        <input id="min-ebc" name="ebc_gt" type="number" min=0 placeholder=0>
        <label for="max-ebc">Max. EBC: </label>
        <input id="max-ebc" name="ebc_lt" type="number" min=0 placeholder=0>
      </div>
      <div class="partition">
        <label for="malt">Malt: </label>
        <select name="malt" id="malt">
          <option value="">Select a malt type</option>
          <option v-for="malt in malts" :value="malt">{{malt}}</option>
        </select>
      </div>
      <div class="partition">
        <label for="hops">Hops: </label>
        <select name="hops" id="hops">
          <option value="">Select a hop type</option>
          <option v-for="hop in hops" :value="hop">{{hop}}</option>
        </select>
      </div>
      <div class="partition">
        <label for="yeast">Yeast: </label>
        <select name="yeast" id="yeast">
          <option value="">Select a yeast type</option>
          <option v-for="yeast in yeasts" :value="yeast">{{yeast}}</option>
        </select>
      </div>
      <input type="submit" value="Search">
    </form>
  </div>
</template>

<script>
import {eventBus} from "../main.js";

export default {
  name: "beer-selector",
  props: [
    "yeasts",
    "malts",
    "hops"
  ],
  methods: {
    handleClick: function(){
      fetch("https://api.punkapi.com/v2/beers/random")
        .then((res) => res.json())
        .then((beer) => eventBus.$emit('beer-selected', beer[0]))
        .then(() => eventBus.$emit('beers-filtered', []))
    },

    handleSubmit: function(evt){
      evt.preventDefault();
      const url = this.buildUrl(evt);
      fetch(url)
        .then((res) => res.json())
        .then((beers) => {if (beers.length > 1){
            eventBus.$emit('beers-filtered', beers)
          }
          else {
            eventBus.$emit('beers-filtered', [])
            eventBus.$emit('beer-selected', beers[0])
          }
        })
    },

    buildUrl: function(evt){
      // declare base url
      let url = "https://api.punkapi.com/v2/beers"

      // get all keys from form and remove the one for the submit button
      const dataKeys = Object.keys(evt.target);
      dataKeys.pop()

      // get elements corresponding to each key
      const mappedElements = dataKeys.map((key) => {
        return evt.target[key]
      })

      // remove elements with no value entered
      const filteredElements = mappedElements.filter((element) => {
        return element.value
      })

      // build url; first check how to append it
      filteredElements.forEach((element, index) => {
        index === 0 ? url += "?" : url += "&"
        url += `${element.name}=${element.value}`
      })

      // Check if url has already had something appended then add the appropriate character
      url += url.includes("?") ? "&" : "?";

      // Set required results to max permitted
      url += "per_page=80";

      return url;
    }
  }
}
</script>


<style lang="css" scoped>
  .partition {
   width: 100%;
  }
</style>