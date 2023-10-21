<template>
  <div>
    <img src="./assets/pokedex.png" alt="Pokedex.png">
    <input class="busca" type="text" placeholder="Digite o nome do pokemon.." v-model="busca">
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
    document.title = 'Vue Pokédex by Noan';
  },
}
</script>

<style>
body {
  min-height: 100vh;
  background: url('./assets/background2.png');
  background-size: 350px 350px;
}
#app {
  text-align: center;
}
.busca {
  border-radius: 12px;
  position: sticky;
  top: 2px;
  z-index: 3;
  width: 94%;
  padding: 8px 6px;
  outline: none;
  border: none;
  box-shadow: 0px 1px 5px black;
}
.content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  grid-gap: 16px;
  margin-inline: 2%;
}
</style>
