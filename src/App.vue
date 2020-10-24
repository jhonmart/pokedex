<template>
  <div id="app" class="container">
    <div class="row">
      <div class="input-field col s6">
        <input value="Alvin" id="first_name2" type="text" class="validate">
        <label class="active" for="first_name2">First Name</label>
      </div>
    </div>
    <div class="row">
      <input type="text" v-model="start" class="col s11">
      <button class="col s1" v-on:click="search">
        <i class="material-icons gren">send</i>
      </button>
    </div>
    <List v-bind:arrayPokemon="pokemons" v-on:select="detail"/>
  </div>
</template>

<script>
// https://pokeapi.co/api/v2/type/
// https://pokeapi.co/api/v2/ability/

import List from './components/PokeList.vue'

export default {
  name: 'App',
  components: {
    List
  },
  data(){
    return {
      pokemons: [],
      types: [],
      start: 0,
      qtdItens: 0
    }
  },
  methods: {
    search(){
      console.log(this.start)
      fetch(`https://pokeapi.co/api/v2/pokemon/?offset=${this.start}&limit=50`)
      .then(el=>el.json())
      .then(el=>{
          this.pokemons = el.results.map(camps=>({
          ...camps,
          id: camps.url.split('/')[6],
          img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${camps.url.split('/')[6]}.svg`
        }))

        this.qtdItens = el.count
      })
    },

    detail(ix){
      console.log('select', ix)
      fetch(`https://pokeapi.co/api/v2/pokemon/${ix}`)
      .then(el=>el.json())
      .then(el=>{
        console.log(el)
      })
    }
  },
  mounted () {
    fetch(`https://pokeapi.co/api/v2/pokemon/?offset=${this.start}&limit=50`)
      .then(el=>el.json())
      .then(el=>{
          this.pokemons = el.results.map(camps=>({
          ...camps,
          id: camps.url.split('/')[6],
          img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${camps.url.split('/')[6]}.svg`
        }))

        this.qtdItens = el.count
      })

      fetch(`https://pokeapi.co/api/v2/type/`)
      .then(el=>el.json())
      .then(el=>{
        this.types = el.results.map(camps=>camps.name)
      })
  }
}
</script>

<style>
  .container{
    height: 720px;
    width: 90%;
    overflow: auto;
  }
  body{
    background-color: rgb(192, 21, 21);
    background: linear-gradient(45deg, #fff, #fcfcfc);
  }
</style>
