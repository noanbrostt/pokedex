<template>
  <article>
    <div class="pokemon" :class="pokemon.mainType">
      <div class="poke-background">
        <div class="nome-numero">
            <span class="nome">{{ priMaiuscula(name) }}</span>
            <span class="numero">#{{ idTresDigitos }}</span>
        </div>
        <img class="pokeball" src="../assets/pokeball.png" alt="Pokedall.png" >
      </div>
      <div class="poke-image">
        <span class="poke-tipos">
          <span class="type" :class="type.type.name" v-for="(type, index) in pokemon.type" :key="index">{{ priMaiuscula(type.type.name) }}</span>
        </span>
        <img class="poke-img" :src="currentImg" :alt="upperName">
      </div>
    </div>
    <img class="hover-image" :src="hoverImg" :alt="priMaiuscula(name)">
  </article>
</template>

<script>
import axios from 'axios'

export default {
  created: function() {
    axios.get(this.url).then(res => {
      // console.log(res.data);
      this.pokemon.id = res.data.id;

      this.pokemon.mainType = res.data.types[0].type.name;
      this.pokemon.type = res.data.types;

      this.pokemon.back = res.data.sprites.back_default;
      this.pokemon.back_female = res.data.sprites.back_female == null ? this.pokemon.back : res.data.sprites.back_female;
      this.pokemon.back_shiny = res.data.sprites.back_shiny;
      this.pokemon.back_shiny_female = res.data.sprites.back_shiny_female == null ? this.pokemon.back_shiny : res.data.sprites.back_shiny_female;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.front_female = res.data.sprites.front_female == null ? this.pokemon.front : res.data.sprites.front_female;
      this.pokemon.front_shiny = res.data.sprites.front_shiny;
      this.pokemon.front_shiny_female = res.data.sprites.front_shiny_female == null ? this.pokemon.front_shiny : res.data.sprites.front_shiny_female;

      this.currentImg = this.pokemon.front;
      this.hoverImg = 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/'+this.pokemon.id+'.svg';
    })
  },
  data() {
    return {
      isFront: true,
      isFemale: false,
      isShiny: false,
      currentImg: '',
      hoverImg: '',
      pokemon: {
        id: '',
        type: Object,
        mainType: '',
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
    },
    priMaiuscula(text) {
      return text[0].toUpperCase() + text.slice(1);
    }
  }
}
</script>

<style>
/*********************/
/*Hover effect begins*/
/*********************/
article {
    position: relative;
    width: 260px;
    transition: all .3s ease;
    margin: 10px auto;
}
article:hover {
    transform: 
        perspective(250px)
        rotateX(10deg)
        translateY(5%)
        translateZ(0);
    z-index: 2;
}
article:hover::before {
    opacity: 1;
}
article::before {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 10px;
    z-index: 2;
    background-image: 
        linear-gradient(
            to bottom,
            transparent 10%,
            rgba(0, 0, 0, 0.5) 50%,
            rgb(0, 0, 0) 95%
        );
        opacity: 0;
        transition: all .3s ease;
}
article .pokemon {
    box-shadow: 0 60px 60px -60px rgba(0, 30, 255, 0.5);
    object-fit: cover;
}
article .hover-image {
    position: absolute;
    max-width: 80%;
    height: 80%;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    transform: translateY(25%);
    transition: .3s ease;
    opacity: 0;
    z-index: 4;
}
article:hover .hover-image {
    opacity: 1;
    transform: translateY(-15%);
}
article:hover .poke-image {
    z-index: unset;
}
article:hover .poke-img {
    opacity: 0.5;
}
article:hover .pokeball {
    opacity: 0.1;
}
/*******************/
/*Hover effect ends*/
/*******************/
.pokemon {
    height: 260px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 8px;
    border-radius: 10px;
}
.nome-numero {
    color: white;
    font-weight: bold;
    font-size: 29px;
    display: flex;
    justify-content: space-between;
}
.poke-image {
    border-radius: 10px;
    background: white;
    display: flex;
    flex-direction: column-reverse;
    align-items: center;
    width: 99%;
    height: 70px;
    padding-bottom: 10px;
    z-index: 2;
    margin-inline: auto;
}
.poke-image::after {
    content: '';
    position: absolute;
    bottom: 55px;
    width: 30%;
    height: 20px;
    border-radius: 50%;
    background: #00000040;
    z-index: 1;
}
.poke-img {
    position: absolute;
    bottom: 20px;
    z-index: 2;
    height: 80%;
}
.pokeball {
    position: absolute;
    top: 30px;
    left: 20px;
    opacity: 0.3;
}
.type {
    color: white;
    padding: 4px 8px;
    border-radius: 10px;
    font-weight: bold;
}
.bug {
    background: #A7B723;
}
.dark {
    background: #75574C;
}
.dragon {
    background: #7037FF;
}
.electric {
    background: #F9CF30;
}
.fairy {
    background: #E69EAC;
}
.fighting {
    background: #C12239;
}
.fire {
    background: #F57D31;
}
.flying {
    background: #A891EC;
}
.ghost {
    background: #70559B;
}
.normal {
    background: #AAA67F;
}
.grass {
    background: #74CB48;
}
.ground {
    background: #DEC16B;
}
.ice {
    background: #9AD6DF;
}
.poison {
    background: #A43E9E;
}
.psychic {
    background: #FB5584;
}
.rock {
    background: #B69E31;
}
.steel {
    background: #B7B9D0;
}
.water {
    background: #6493EB;
}
</style>