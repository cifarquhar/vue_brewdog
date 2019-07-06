<template lang="html">
  <div class="card" v-on:click="handleClick">
    <div class="row no-gutters">
      <div class="col-md-4">
        <img class="card-img" :src="beer.image_url" :alt="beer.name">
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h2>{{beer.name}}</h2>
          <h4>{{beer.tagline}}</h4>
          <p><b>ABV:</b> {{beer.abv}}</p>
          <p><b>IBU:</b> {{beer.ibu}}</p>
          <p><b>EBC:</b> {{beer.ebc}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {eventBus} from "../main.js";

export default {
  name: "beer",
  props: ["beer"],
  methods: {
    handleClick: function(){
      eventBus.$emit('beer-selected', this.beer)
      eventBus.$emit('modal-state-change', true)
    }
  }
}
</script>

<style lang="css" scoped>

</style>


