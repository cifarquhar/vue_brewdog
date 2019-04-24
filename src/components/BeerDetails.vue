<template lang="html">
  <div v-if="beer">
    <h2>{{beer.name}}</h2>
    <h4>{{beer.tagline}}</h4>
    <p>{{beer.description}}</p>
    <img v-if="beer.image_url" :src="beer.image_url" :alt="beer.name" class="beer-image">
  </div>
</template>

<script>
export default {
  name: "beer-details",
  props: ["beer"]
}
</script>

<style lang="css" scoped>
  .beer-image{
    height: 100px;
    width: auto;
  }
</style>


