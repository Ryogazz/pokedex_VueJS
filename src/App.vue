<template>
  <v-app>
    <v-container>
      <v-card>
       <v-container>
         <v-row>
          <v-col 
            cols="3" 
            v-for="poke in pokemon" 
            :key="poke.name"
            >
            <h2>
              {{ poke.name }}
            </h2>
          </v-col>
         </v-row>

       </v-container>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
import axios from 'axios';
export default { 
  name: 'App',

  components: {

  },

  data() {
    return {
      pokemon: []
    }
  },

  mounted () {
  axios.get('https://pokeapi.co/api/v2/pokemon?limit=151').then((response) => {
    this.pokemon = response.data.results
  })
},

};


</script>


<style>
#app {
  background: linear-gradient(
      to bottom right,
      rgb(64, 20, 20),
      rgb(1, 35, 64)
    )
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>