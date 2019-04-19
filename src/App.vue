<template>
  <div id="app">
    <ul>
      <li v-for="(beer, index) in allBeers" :key="index">{{beer.name}}</li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return{
      allBeers: []
    }
  },
  mounted(){
    fetch("https://api.punkapi.com/v2/beers")
      .then((res) => res.json())
      .then((data) => this.allBeers = data);
  }
}
</script>

<style>

</style>
