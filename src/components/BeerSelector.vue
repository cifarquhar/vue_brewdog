<template lang="html">
  <div>
    <button v-on:click="handleClick">Get Random Beer</button>
    <form v-on:submit="handleSubmit">
      <label for="name">Name: </label>
      <input id="name" name="beer_name" type="text">
      <label for="min-abv">Min. ABV: </label>
      <input id="min-abv" name="abv_gt" type="number" min=0 placeholder=0>
      <label for="max-abv">Max. ABV: </label>
      <input id="max-abv" name="abv_lt" type="number" min=0 placeholder=0>
      <label for="min-ibu">Min. IBU: </label>
      <input id="min-ibu" name="ibu_gt" type="number" min=0 placeholder=0>
      <label for="max-ibu">Max. IBU: </label>
      <input id="max-ibu" name="ibu_lt" type="number" min=0 placeholder=0>
      <label for="min-ebc">Min. EBC: </label>
      <input id="min-ebc" name="ebc_gt" type="number" min=0 placeholder=0>
      <label for="max-ebc">Max. EBC: </label>
      <input id="max-ebc" name="ebc_lt" type="number" min=0 placeholder=0>
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
        .then((beers) => eventBus.$emit('beers-filtered', beers))
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

      return url;
    }
  }
}
</script>


<style lang="css" scoped>

</style>