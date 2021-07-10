<template>
  <div class="home">
    <h1>Generátor receptů</h1>
    <div v-if="this.rnd != undefined"> Jídlo dne: {{ this.jidlo[this.rnd].name }}</div>
    <div> Číslo dne: {{ this.rnd + 1}} z {{ getFoodLength }}</div>
    <br>
    <b-btn-group>
      <b-btn
          @click="newRnd"
      >Generuj nové </b-btn>
      <b-btn
          @click="toggleForm = true"
      >Hledej podle surovin</b-btn>
    </b-btn-group>
    <div v-show="toggleForm">
      <input v-model="ing">
      <b-btn @click="available">Hledej</b-btn>
        <b-list-group
          :style="{
              'align-items':'center'
          }">
          <b-list-group-item v-for="food in readyToCook"
             :style="{
              'text-align':'left',
              'width':'200px'
            }"
            @click="selectedFood = food; separateIng()">
            {{ food.name }}
          </b-list-group-item>
        </b-list-group>
      <br>
      <div class="collection" v-if="selectedFood">
        <h3>{{ selectedFood.name }}</h3>
        <p> {{ selectedFood.suroviny.join(', ')}} </p>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { mapGetters } from 'vuex'

export default {
  name: 'Home',
  components: {

  },
  data () {
    return {
      rnd: null,
      ing: '',
      readyToCook: [],
      toggleForm: false,
      selectedFood: null,
      recept: ''
    }

  },
  computed: {
    ...mapGetters([
       'getFoodLength'
    ]),
    jidlo() {
      return this.$store.state.food;
    },
  },
  methods: {
    newRnd(){
      this.rnd = Math.ceil(Math.random()*this.jidlo.length - 1)
    },
    available() {
      let fcount = Object.keys(this.jidlo).length
      for (let i = 0; i < fcount; i++) {
        for (let j = 0; j < this.jidlo[i].suroviny.length; j++) {
          if (this.jidlo[i].suroviny[j] === this.ing && this.readyToCook.indexOf(this.jidlo[i].name) === -1){
            this.readyToCook.push(this.jidlo[i])
          }
        }
      }
    }
  },
  mounted() {
    this.rnd=Math.ceil(Math.random()*this.jidlo.length - 1)
  }
}
</script>
<style>
.btn {
  margin: 20px;
}
.collection {
  /* these styles are optional here, you might not need/want them */
  margin-top: -1px;
  margin-left: 30%;
  width: 40%;
}

.collection .child {
  outline: 1px solid; /* use instead of border */
  margin-top: 1px;
  margin-left: 1px;
  padding: 10px;
}

</style>