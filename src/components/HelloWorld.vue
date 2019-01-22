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
      <div class="botonshowpoke"> <button style="color: black" @click="verPoke()">Show Pokemon</button>  </div>
        Pokemon con ID aleatorio
      <img v-if="isPokemonShown" :src="pokemon.image"/>

      <pre v-if="isPokemonShown">
        {{pokemon.name}}
        {{pokemon.id}}
        {{pokemon.move}}
      </pre>
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
      image: '',
      name: '',
      id: '',
      move: '',
      code:'',
      msg: '',
      isPokemonShown: false,
      pokemon: {}
    }
  },
  methods: {
    async verPoke(){
      this.isPokemonShown = true;
    },
    async sendID(){
      (this.mandoID == '')
        ? alert('Favor de ingresar un ID')
        : (this.mandoID <= 802)
          ? this.pokemon = this.getPokemonDataFromResponse(await axios.post('http://localhost:6001/verPokemones',{id: this.mandoID}))
          : alert(`El pokemon con id ${this.mandoID} no está registrado en la Pokedex`)
    },
    getPokemonDataFromResponse(response){
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
pre{
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
