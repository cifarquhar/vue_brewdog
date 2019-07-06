<template lang="html">
<transition name="modal-fade">
  <div v-show="showModal" class="modal-backdrop">
  <div class="modal" role="dialog" aria-labelledBy="modalTitle" aria-describedBy="modalDescription">

    <header class="modal-header" id="modalTitle">
    <slot name="header">
      <h2>{{beer.name}}</h2>
    </slot>
    <slot name ="details">
      <p>
        <b>ABV:</b> {{beer.abv}}
        <b>IBU:</b> {{beer.ibu}}
        <b>EBC:</b> {{beer.ebc}}
      </p>
      <h4>{{beer.tagline}}</h4>
      <p>{{beer.description}}</p>
      <img v-if="beer.image_url" :src="beer.image_url" :alt="beer.name" class="beer-image">
    </slot>
    </header>

    <section class="modal-body" id="modalDescription">
    <slot name="ingredients" >
      <h4>Ingredients:</h4>
      <div class="modal-content">
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
      </div>
      </slot>
      <slot name="method">
        <h4>Method:</h4>
      <div class="modal-content">
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
      <p v-if="beer.method.twist" id="twist-p"><i>To add something extra:</i> {{beer.method.twist}}</p>
    </div>
    </slot>
    </section>

    <footer class="modal-footer">
      <slot name="footer">
        <button
          type="button"
          class="btn-green"
          v-on:click="close"
          aria-label="Close modal"
            >
              Close
            </button>
      </slot>
    </footer>
  </div>
  </div>
  </transition>
</template>

<script>
import { eventBus } from '../main';
export default {
  name: "beer-details",
  props: ["beer", "showModal"],
  methods: {
    close: function(){
      eventBus.$emit("modal-state-change", false)
    }
  }
}
</script>

<style lang="css" scoped>
  .beer-image{
    height: 100px;
    width: auto;
  }
  th > td{
    text-align:center;
    border-bottom: 1px solid grey;
  }
    .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: scroll;
    display: flex;
    flex-direction: column;
    margin: 10%;
    height: 80%;
    width: 80%;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
    flex-direction: column;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
  }

  .modal-content {
    flex-direction: row;
    justify-content: space-evenly;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }

  #twist-p {
    width: 50%;
  }
</style>


