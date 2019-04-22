<template lang="html">
  <div>
    <button v-on:click="handleClick">Get Random Beer</button>
  </div>
</template>

<script>
import {eventBus} from "../main.js";

export default {
  name: "beer-selector",
  methods: {
    handleClick: function(){
      fetch("https://api.punkapi.com/v2/beers/random")
        .then((res) => res.json())
        .then((beer) => eventBus.$emit('beer-selected', beer[0]))
    }
  }
}
</script>


<style lang="css" scoped>

</style>