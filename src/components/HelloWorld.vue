<template>
  <div class="hello">

    <div class="barraBusqueda">
      <h3>Ingrese el ID del pokemon:</h3>
      <input style="color: black" type="text" v-model="mandoID" maxlength="3" @keyup.enter="sendID()">
    </div>

    <div class="cantidad">
      <h3>¿Cuántos Pokemons quieres ver?</h3>
      <input type="number" v-model="limit" maxlength="3" @keyup.enter="pokeInterval()">{{limit}}
    </div>

    <div class="pokealeatorio">

      <div class="botonshowpoke"> <button style="color: black" @click="pokeRandom()">Show Pokemon Random</button>  </div>

      <p v-if="isPokemonShown">
        <br>Pokemon con ID aleatorio <br>
        <img :src="pokemon.image"/>
        <br>Un {{pokemon.name}} ha aparecido<br>
        El id del pokemon es: {{pokemon.id}}<br>
        ¡Cuidado!, {{pokemon.name}} ha usado: {{pokemon.move}}<br>
      </p>
    </div>

    <div class="pokeID">

      <p v-if="isPokemonSpec">
        <br>Pokemon con ID Específico <br>
      <img :src="pokemon2.image"/>
        <br>Un {{pokemon2.name}} ha aparecido<br>
        El id del {{pokemon2.name}} es: {{pokemon2.id}}<br>
        ¡Cuidado!, {{pokemon2.name}} ha usado: {{pokemon2.move}}<br>


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
      isPokemonShown: false,
      isPokemonSpec: false,
      pokemon: {},
      pokemon2: {},
      prueba: {}
    }
  },
  methods: {

    async pokeRandom(){
      this.isPokemonShown = true;
       this.pokemon = this.getPokemonDataFromResponse (await axios.post('http://localhost:6001/verPokemones') );
    },

    getPokemonDataFromResponse(response){
      // eslint-disable-next-line
      console.log(response);
      return response.data;

    },

    async sendID(){
      this.isPokemonSpec = true;
      (this.mandoID == '')
        ? alert('Favor de ingresar un ID')
        : (this.mandoID <= 802)
          ? this.pokemon2 = this.getPokemonDataFromResponse( await axios.get('http://localhost:6001/pokemonEspec', { params:{ id : this.mandoID } }))
          : alert(`El pokemon con id ${this.mandoID} no está registrado en la Pokedex`);
    },

    async pokeInterval(){
      (this.intervalo == '')
        ? alert('Favor de ingresar un Intervalo')
        : this.back()
    },
    async back(){
      this.prueba = this.getPokemonDataFromResponse( await axios.get('http://localhost:6001/pokemonIntervalo', { params:{limit: this.limit, offset: this.mandoID}}))
    },

  }
}
</script>


<style scoped>
button {
  text-align: center;
  margin: 40px 0 0;
  background-color: rgb(9, 173, 238);
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
.pokeID{
  margin-top: 128px;
  position: absolute;
  top: 150px;
  left: 50%;
}
</style>
