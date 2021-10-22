<template>
  <div id="app"> 
    <div class="column is-half is-offset-one-quarter">
      <h4 class="is-size-4">Autor: Juanzcc</h4>
      <hr>
      <img src="./assets/pokedex.png">
      <hr>
      <input type="text" placeholder="Buscar pokemon pelo nome" v-model="busca" class="input is-rounded">
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url"> 
        <pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import pokemon from './components/pokemon'

export default {
  name: 'App',
  data(){
      return{
        pokemons:[],
        filteredPokemons: [],
        busca: ''
      }
  },
  created: function(){ //Quando o site carregar esta função diretamente
    //req de get para pegar dados da api de pokemon
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      console.log("Pegou a lista de Pokemons!!")
      this.pokemons = res.data.results //passando a lista para minha arry pokemons
      this.filteredPokemons = res.data.results
    })
  },
  components:{
    pokemon
  },
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  },
  computed:{
    /*
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
    */
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#buscaBtn{
  margin-top: 2%;
}

</style>
