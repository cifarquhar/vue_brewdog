<template lang="html">
  <div>
    <h2>{{beer.name}}</h2>
    <h3>{{beer.tagline}}</h3>
    <p>{{beer.description}}</p>
    <img :src="beer.image_url" :alt="beer.name" class="beer-image">
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


