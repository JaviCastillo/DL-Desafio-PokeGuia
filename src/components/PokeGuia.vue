<template>
  <div>
      <img src="https://i.pinimg.com/originals/d8/43/ad/d843addbfcec31846d8699feebcf358b.png" alt="pokemon-logo">
      <h1>PokeGuía</h1>
      <form @submit.prevent="obtenerDatos(pokenombre)">
          Nombre:
          <input type="text" v-model="pokenombre">
          <button type="submit">Buscar</button>
      </form>
      <img :src="traerImagen" alt="">
      <h2>Movimientos</h2>
      <ul>
          <li v-for="(item, index) in traerMovimientos" :key="index">{{item}}</li>
      </ul>
      <h2>Habilidades</h2>
      <ul>
          <li v-for="(item, index) in traerHabilidades" :key="index">{{item}}</li>
      </ul>
  </div>

</template>

<script>
import axios from 'axios';

export default {
    name: 'PokeGuia',
    data() {
        return {
            pokenombre: '',
            imagen: '',
            movimientos: [],
            habilidades: [],
        }
    },
    methods: {
        obtenerDatos(pokemon){
            if(pokemon){
                axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemon.toLowerCase()}`)
                .then(json => {
                    this.pokenombre = json.data.name 
                    this.imagen = json.data.sprites.front_default
                    this.movimientos = json.data.moves
                    this.habilidades = json.data.abilities
                    })
                .catch(err => {
                    if (err.response) {
                      alert('Pokemon no encontrado')
                    } else if (err.request) {
                      alert('PokeApi no responde')
                    } else {
                      console.log(err)
                    }
                  })
            }
        }
    },
    computed: {
        /* Hacer cosas especificas */
        traerImagen(){
            return this.imagen
        },
        traerMovimientos(){
            let nombresMov = []
            this.movimientos.forEach(element => {
               nombresMov.push(element.move.name)
            });
            return nombresMov
        },
        traerHabilidades(){
            let nombresHab = []
            this.habilidades.forEach(element => {
               nombresHab.push(element.ability.name)
            });
            return nombresHab
        }
    },
    created() {
        this.obtenerDatos('pikachu');
    },
}
</script>

<style>
    *{
        text-align: center;
    }
    li {
        list-style:none;
    }
</style>