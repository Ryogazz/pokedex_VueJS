<template>
  <v-app>
    <v-container>
     
       <v-container >
        <v-row>
          <v-container>
            <v-img
              :src="require('../src/assets/pokedex.png')"
              class="my-3"
              contain
              height="200"
            />
            <h1 class="text-center white--text mb-2" style="font-size: 5rem">
              Pokedex
            </h1>
          </v-container>
        </v-row>
          <v-text-field
            label="Digite o nome do pokemon"
            placeholder="Pikachu"
            solo
            v-model="buscar"
          ></v-text-field>
         <v-row
         v-if="buscar.length > 0"
         >
          <v-col 
          
            v-for="poke in filtro_busca_pokemon"
            :key="poke.name"
            >
            <v-card @click="mostrar_poke(get_id(poke))">
              <v-container >
                <v-row class="mx-0 d-flex justify-center">
                  <img 
                  :src="
                `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${ 
                  get_id(poke) }.gif`" 
                :alt="poke.name"
                width="70px"
                height="70px"

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
    </v-container>

    <v-dialog
      v-model="open_dialog"
      width="500"
    >
      <v-card v-if="selecionado" class="px-4">
        <v-container>
          <v-row class="d-flex aling-center">
            <v-col cols="4">
              <img 
                  :src="
                `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${selecionado.id}.gif`" 
                :alt="selecionado.name"
                width="70px"
                height="70px"
                />
            </v-col>
            <v-col cols="8">
              <h1>{{ get_name(selecionado) }}</h1>
              <v-chip 
                v-for="type in selecionado.types" 
                :key="type.slot"
                class="ml-2"
                >{{ type.type.name }}
              </v-chip>
              <v-divider class="my-4"></v-divider>
              <v-chip>Altura {{ selecionado.height * 2.54}} cm</v-chip>
              <v-chip class="ml-2"
              >Peso 
              {{ 
                (selecionado.weight * 0.45359237).toFixed(0)
              }} 
              kgs</v-chip>
            </v-col>
          </v-row>
          <h3>Status</h3>
            <v-row>
              <v-col
                cols="12"
                md="4"
                v-for="stat in selecionado.stats"
                :key="stat.stat.name"
              >
                <v-chip
                  style="width: 100%"
                  class="d-flex justify-center text-center black--text"
                >
                  {{ (stat.stat.name) + ' ' +  (stat.base_stat) }}
                </v-chip>
              </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-dialog>
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
      open_dialog: false,
      selecionado: null,
      evolution: ''
    }
  },

  mounted () {
  axios
    .get('https://pokeapi.co/api/v2/pokemon?limit=600')
    .then((response) => {
    this.pokemon = response.data.results
  })
},

methods: {
  get_id(pokemon){
    return Number(pokemon.url.split('/')[6]);
  },
  get_name(pokemon){
    return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)
  },
  mostrar_poke(id){
    axios
    .get(`https://pokeapi.co/api/v2/pokemon/${id}`)
    .then((response) => {
    this.selecionado = response.data
    this.open_dialog = !this.open_dialog
  })
  },
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