<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pixelpokeball.png">
      <hr>
      <h4 class="is-size-3">Pokedex!</h4>
      <hr>
      <input class="input is-rounded" type="text" name="" id="" placeholder="search pokemon by name!" v-model="searchParam">
      <button class="button is-fullwidth is-warning" id="search-button" @click="searchPokemons">search!</button>
      <br>
      <br>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
          <Pokemon :num="index + 1" :name="poke.name" :url="poke.url"></Pokemon>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import Pokemon from "./components/Pokemon"

export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      searchParam: ""
    }
  },
  //método chamado quando o componente é criado
  created: async function(){
    const response = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0");
    this.pokemons = response.data.results;
    this.filteredPokemons = response.data.results;
  },
  components: {
    Pokemon
  },
  filters: {
    
  },
  methods:{
    searchPokemons: function(){
      if(this.searchParam.trim() == ""){
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(p=> p.name.toUpperCase().includes(this.searchParam.toUpperCase().trim()))
      }
    }
  },
  computed:{
    /*
    searchResult: function(){
      console.log(this.searchParam);
      if(this.searchParam.trim() == ""){
        return this.pokemons;
      } else {
        return this.pokemons.filter(s=> s.name.includes(this.searchParam));
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
#search-button{
  margin-top: 2%;
}
</style>
