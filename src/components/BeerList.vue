<template lang="html">
  <ul>
    <beer v-for="(beer, index) in beers" :beer="beer" :key="index"></beer>
  </ul>
</template>

<script>
import Beer from "./Beer.vue"

export default {
  name: "beer-list",
  props: ["beers"],
  components: {
    "beer": Beer
  }
}
</script>

<style lang="css" scoped>

</style>


