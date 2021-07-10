<template>
  <div class="knihovna">
    <h1>Kniha receptů</h1>

    <b-row v-for="chunk in foodChunks">

      <b-col v-for="food in chunk">
        {{ food.name }}
      </b-col>

    </b-row>
  </div>
</template>

<script>
 export default {
   computed: {
     jidlo () {
       return this.$store.state.food
     },
     foodChunks(){
       return _.chunk(this.jidlo, 3);
     }
   }
 }
</script>
<style>

h1 {
  margin-bottom: 50px;
}

.col {
  margin: 40px 40px;
  font-weight: 700;font-size: 40px;font-style: normal;
}

</style>