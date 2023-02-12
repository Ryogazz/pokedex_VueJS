<template>
  <v-app>
    <v-container>
      <v-card>
       <v-container>
          <v-text-field
            label="Digite o nome do pokemon"
            placeholder="Pikachu"
            solo
            v-model="buscar"
          ></v-text-field>
         <v-row>
          <v-col 
            cols="2" 
            v-for="poke in filtro_busca_pokemon" 
            :key="poke.name"
            >
            <v-card>
              <v-container>
                <v-row class="mx-0 d-flex justify-center">
                  <img 
                  :src="
                `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${ 
                  get_id(poke) }.gif`" 
                :alt="poke.name"
                width="50%"
                />
                </v-row>
                <h2 class="text-center">
                  {{ get_name(poke) }}
                </h2>
              </v-container>
            </v-card>
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
      pokemon: [],
      buscar: '', 
    }
  },

  mounted () {
  axios.get('https://pokeapi.co/api/v2/pokemon?limit=151').then((response) => {
    this.pokemon = response.data.results
  })
},

methods: {
  get_id(pokemon){
    return Number(pokemon.url.split('/')[6]);
  },
  get_name(pokemon){
    return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)
  }
},

computed: {
  filtro_busca_pokemon() {
    return this.pokemon.filter((item) => {
      return item.name.includes(this.buscar)
    })
  }
}

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