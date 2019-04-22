<template lang="html">
  <div>
    <button v-on:click="handleClick">Get Random Beer</button>
    <form v-on:submit="handleSubmit">
      <label for="name">Name: </label><input id="name" name="name" type="text">
      <input type="submit" value="Search">
    </form>
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
    },
    handleSubmit: function(evt){
      evt.preventDefault();
      console.log("form submitted");
    }
  }
}
</script>


<style lang="css" scoped>

</style>