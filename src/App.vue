<template>
  <div class="">
    <img src="./assets/pokedex.png" alt="Pokedex.png">
    <input class="input is-rounded" type="text" placeholder="Digite o nome do pokemon.." v-model="busca">
    <div class="content">
      <PokeDiv v-for="(poke) in resultadoBusca" :key="poke.url" :name="poke.name" :url="poke.url"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import PokeDiv from './components/PokeDiv'

export default {
  name: 'App',
  components: {
    PokeDiv
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  computed: {
    resultadoBusca: function() {
      if(this.busca.length == 0){
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name.indexOf(this.busca.toLowerCase()) > -1);
      }
    }
  },
  mounted() {
    // Define o título da página no momento em que o componente é montado
    document.title = 'Pokédex by Noan Brostt';
  },
}
</script>

<style>
#app {
  text-align: center;
}
#buscarBtn {
  margin-top: 15px;
}
.content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  grid-gap: 16px;
  margin-inline: 30px;
}
</style>
