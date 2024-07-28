<template>
    <div class="pokemon-card">
        <img :src="poke.img" alt="img pokemon" :class="{filtro : ! poke.descubierto}">
        <h3 v-show="poke.descubierto" >{{ poke.name  }}</h3>
        <input type="text" v-show="!poke.descubierto"  v-model="nombre">
        <button v-show="!poke.descubierto"
                @click="$emit('validarPokemon',{nombre,indice})"
        >Descubrir</button>
    </div>
</template>

<script>
export default{
  props:["poke","indice"],
  data(){
    return{
        nombre:"",
    }
  }
}
</script>
<style>
    .filtro { 
            filter:blur(5px) grayscale(100%);
    }
.pokemon-card {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  input,
  button {
    width: 100px;
  }
}
</style>
