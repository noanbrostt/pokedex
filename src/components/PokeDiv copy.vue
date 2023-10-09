<template>
  <div :class="{'pokemon': true, 'hidden': pokemon.id > 4}">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image">
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">#{{ idTresDigitos }} {{ upperName }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>
        <div class="content">
          <button class="button is-medium" @click="mudarSentido">Girar</button>
          <button class="button is-medium" @click="mudarSexo">Sexo</button>
          <button class="button is-medium" @click="mudarShiny">Shiny</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created: function() {
    axios.get(this.url).then(res => {
      // console.log(res.data);
      this.pokemon.id = res.data.id;
      for (let i = 0; i < res.data.types.length; i++) {
        this.pokemon.type+= res.data.types[i].type.name + ' ';        
      }
      // this.pokemon.type = res.data.types[0].type.name;

      this.pokemon.back = res.data.sprites.back_default;
      this.pokemon.back_female = res.data.sprites.back_female == null ? this.pokemon.back : res.data.sprites.back_female;
      this.pokemon.back_shiny = res.data.sprites.back_shiny;
      this.pokemon.back_shiny_female = res.data.sprites.back_shiny_female == null ? this.pokemon.back_shiny : res.data.sprites.back_shiny_female;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.front_female = res.data.sprites.front_female == null ? this.pokemon.front : res.data.sprites.front_female;
      this.pokemon.front_shiny = res.data.sprites.front_shiny;
      this.pokemon.front_shiny_female = res.data.sprites.front_shiny_female == null ? this.pokemon.front_shiny : res.data.sprites.front_shiny_female;

      this.currentImg = this.pokemon.front;
    })
  },
  data() {
    return {
      isFront: true,
      isFemale: false,
      isShiny: false,
      currentImg: '',
      pokemon: {
        id: '',
        type: '',
        front: '',
        back: ''
      }
    }
  },
  props: {
      name: String,
      url: String
  },
  computed: {
    upperName: function() {
      return this.name[0].toUpperCase() + this.name.slice(1);
    },
    idTresDigitos: function() {
      return ('00' + this.pokemon.id).slice(-3);
    }
  },
  methods: {
    mudarSprite: function() {
      var front = this.isFront ? 'front' : 'back';
      var shiny = this.isShiny ? '_shiny' : '';
      var female = this.isFemale ? '_female' : '';

      this.currentImg = this.pokemon[front+shiny+female];
    },
    mudarSentido: function() {
      this.isFront = !this.isFront;
      this.mudarSprite();
    },
    mudarSexo: function() {
      this.isFemale = !this.isFemale;
      this.mudarSprite();
    },
    mudarShiny: function() {
      this.isShiny = !this.isShiny;
      this.mudarSprite();
    }
  }
}
</script>

<style>
.pokemon {
  display: grid;
  margin-top: 15px;
}
</style>