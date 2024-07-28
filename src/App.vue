<template>
    <div id="app">
    <img id="pokemon" src="./assets/logo-pokemon.png" alt="pokemon">
   <h1>¿Quién es ese Pokémon?</h1>
   <h2>Pokemones descubierto: {{ cantidadDescubiertos }}</h2>
   <h2>Cantidad de acierto: {{ cantidadAciertos}}</h2>
   
   <div class="poke">
   <div v-for="(pokemon,i) in pokemones" :key="i">
    <PokeCard :poke="pokemon" 
              :indice="i" 
              @validarPokemon="validarPokemon"
              />
   </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios'
import PokeCard from './components/PokeCard.vue'

export default {
  name: 'App',
  components:{
    PokeCard,
  },
  data(){
    return{
      pokemones:[],
    }
  },
  methods:{
    async obtenerListaPokemones(){
      const url="https://pokeapi.co/api/v2/pokemon";
      const {data} = await axios.get(url);
      const lista = data.results;
      for(const poke of lista){
        this.obtenerPokemon(poke.url);
      }
    },
    async obtenerPokemon(url){
      const {data} = await axios.get(url);
      const name = data.name;
      const img = data.sprites.other.dream_world.front_default;
      const descubierto = false;
      this.pokemones.push({name,img,descubierto});
      console.log(this.pokemones.length);
    },
    validarPokemon({nombre,indice}){
      
      if(this.pokemones[indice].name == nombre)
        this.pokemones[indice].descubierto = true;
      else
        alert("Nombre incorrecto");

    }

  },
  mounted(){
    this.obtenerListaPokemones();
  },
  computed:{
    cantidadDescubiertos(){
      let cantidad = 0;
      for(let i = 0 ; i< this.pokemones.length;i++){
        if(this.pokemones[i].descubierto == true)
            cantidad++;
      }
      return cantidad;
    },
    cantidadAciertos(){
      const pokeAciertos = this.pokemones.filter((poke) => poke.descubierto == true)
      return pokeAciertos.length;
    }

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

.poke {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 10px;
}
.poke img {
  width: 100px; /* Ajusta este valor según el tamaño deseado */
  height: 100px; /* Ajusta este valor según el tamaño deseado */
  object-fit: contain; /* Mantiene la relación de aspecto */
}

#pokemon {
  width: 200px; /* Ajusta este valor según el tamaño deseado */
  height: auto; /* Mantiene la relación de aspecto */
  margin-bottom: 20px;
}
</style>
