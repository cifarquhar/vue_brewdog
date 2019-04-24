<template lang="html">
  <div v-if="beer">
    <h2>{{beer.name}}</h2>
    <p>
      <b>ABV:</b> {{beer.abv}}
      <b>IBU:</b> {{beer.ibu}}
      <b>EBC:</b> {{beer.ebc}}
    </p>
    <h4>{{beer.tagline}}</h4>
    <p>{{beer.description}}</p>
    <img v-if="beer.image_url" :src="beer.image_url" :alt="beer.name" class="beer-image">
    <table>
      <th>
        <td>Grain</td>
        <td>Quantity</td>
      </th>
      <tr v-for="(ingredient, index) in beer.ingredients.malt" :key="index">
        <td>{{ingredient.name}}</td>
        <td>{{`${ingredient.amount.value} kg`}}</td>
      </tr>
    </table>
    <table>
      <th>
        <td>Hop</td>
        <td>Quantity</td>
        <td>Time</td>
      </th>
      <tr v-for="(hop, index) in beer.ingredients.hops" :key="index">
        <td>{{hop.name}}</td>
        <td>{{`${hop.amount.value} g`}}</td>
        <td>{{hop.add}}</td>
      </tr>
    </table>
    <p><b>Yeast:</b> {{beer.ingredients.yeast}}</p>
    <table>
      <th>
        <td>Mash:</td>
      </th>
      <tr v-for="(stage, index) in beer.method.mash_temp">
        <td>{{stage.duration ? stage.duration : 60}} mins</td>
        <td>{{stage.temp.value}} &#8451;</td>
      </tr>
    </table>
    <p><b>Ferment:</b> {{beer.method.fermentation.temp.value}} &#8451;</p>
    <p v-if="beer.method.twist"><i>To add something extra:</i> {{beer.method.twist}}</p>
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


