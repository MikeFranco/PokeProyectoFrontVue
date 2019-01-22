<template>
  <div class="hello">

    <div class="barraBusqueda">
      <h3>Ingrese el ID del pokemon:</h3>
      <input style="color: black" type="text" v-model="mandoID" maxlength="3" @keyup.enter="sendID()">
      {{mandoID}}
    </div>

    <div class="cantidad">
      <h3>¿Cuántos Pokemons quieres ver?</h3>
      <input type="text" v-model="limit" maxlength="3" >
    </div>

    <div class="pokealeatorio">
      <div class="botonshowpoke"> <button style="color: black" @click="pokeRandom()">Show Pokemon</button>  </div>
        <br>Pokemon con ID aleatorio <br>
      <img v-if="isPokemonShown" :src="pokemon.image"/>

      <p v-if="isPokemonShown">
        Un {{pokemon.name}} salvaje ha aparecido<br>
        ID del pokemon: {{pokemon.id}}<br>
        ¡Cuidado!, {{pokemon.name}} ha usado: {{pokemon.move}}<br>
      </p>
    </div>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data(){
    return {
      mandoID: '',
      limit: '',
      msg: '',
      isPokemonShown: false,
      pokemon: {},
      ver: ''
    }
  },
  methods: {
    async pokeRandom(){
      this.isPokemonShown = true;
       this.pokemon = this.getPokemonDataFromResponse (await axios.post('http://localhost:6001/verPokemones') );
       //alert('Esto se murió')
    },
    getPokemonDataFromResponse(response){
      // eslint-disable-next-line
      console.log(response);
      return response.data;

    }

  }
}
</script>


<style scoped>
button {
  text-align: center;
  margin: 40px 0 0;
  background-color: rgb(76, 129, 226);
  border-radius: 10px;
}
p{
  text-align:center;
}
.barraBusqueda{
  text-align: start;
  position: absolute;
  top: 0px;
  left: 350px;
}
.cantidad{
  text-align: start;
  position: absolute;
  top: 0px;
  left: 60%;
}
.botonshowpoke{
  padding-top: 50px;
}
input{
  border-radius: 15px;
}
.pokealeatorio{
  position: absolute;
  top: 150px;
  left: 150px;
}
</style>
