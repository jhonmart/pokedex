<template>
  <div id="app" class="container">
    <div id="list">
      <Search v-on:search="result"/>
      <List v-bind:arrayPokemon="pokemons" v-on:select="detail"/>

      <Pagination 
        v-bind:totalPages="totalPages"
        v-bind:currentPage="currentPage"
        v-bind:pageChangeHandler="pageChangeHandler"/>
    </div>

    <ModalDetails v-bind:pokemon="pokemonSelect"/>
  </div>
</template>

<script>
import List from './components/List.vue'
import Search from './components/Search.vue'
import Pagination from './components/Pagination.vue'
import ModalDetails from './components/ModalDetails.vue'

export default {
  name: 'App',
  components: {
    List,
    Search,
    ModalDetails,
    Pagination
  },
  data(){
    return {
      noPagePokemons: [{
        id: 1,
        name: '',
        img: ''
      }],
      pokemons: [{
        id: 1,
        name: '',
        img: ''
      }],
      pokemonSelect: {
        name: '',
        abilities: [{ability: {name: ""}}],
        id : 0
      },

      currentPage: 1,
      totalPages: 0,

      start: 0,
      qtdView: 24,

      typeSearch: 'https://pokeapi.co/api/v2/pokemon/'
    }
  },
  methods: {
    search(start = this.start){
      fetch(`${this.typeSearch}?offset=${start}&limit=${this.qtdView}`)
      .then(el=>{
        if(el.status === 404){
          return {
            id: 404,
            name: 'Luxio',
            abilities: 1
          }
        } else return el.json()
      })
      .then(el=>{
        let listItens = (el.results || el.pokemon)
        if(el.abilities && !listItens){
          this.pokemons = [{
            id: el.id,
            name: el.name,
            url: 'https://pokeapi.co/api/v2/pokemon/'+ el.id,
            img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${el.id}.svg`
          }]

          this.noPagePokemons = []
          this.totalPages = 0
        } else{
          let exempleList = listItens.map(camps=>({
            name: camps.pokemon? camps.pokemon.name : camps.name,
            id: (camps.url || camps.pokemon.url).split('/')[6],
            img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${(camps.url || camps.pokemon.url).split('/')[6]}.svg`
          })) || []

          
          if(exempleList.length>this.qtdView){
            this.noPagePokemons = exempleList
            this.pokemons = exempleList.slice(0, this.qtdView)
            this.totalPages = Math.ceil(exempleList.length/this.qtdView)
          } else{
            this.pokemons = exempleList
            this.noPagePokemons = []
            this.totalPages = Math.ceil(el.count/this.qtdView)
          }

          this.currentPage = 1
        }
      })
      .catch(err=>{
        console.log(err)
        alert('There was an unexpected error')
      })
    },
    
    selectInnerPage(page){
      console.log('page', page)
      this.pokemons = this.noPagePokemons.slice(this.qtdView * (page - 1), this.qtdView * page)
    },

    pageChangeHandler(selectedPage) {
      this.currentPage = selectedPage
      console.log('change page', this.noPagePokemons.length, this.pokemons.length)
      if(this.noPagePokemons.length > this.pokemons.length){
        this.selectInnerPage(selectedPage)
      } else{
        this.search(this.qtdView*(selectedPage-1))
      }
    },

    detail(ix){
      fetch(`https://pokeapi.co/api/v2/pokemon/${ix}`)
      .then(el=>el.json())
      .then(el=>{
        el.img = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${ix}.svg`
        this.pokemonSelect = el
        document.querySelector('#modal-detail').style.display = 'block'
        document.querySelector('.modal-overlay').style.display = 'block'
        document.querySelector('#list').style.filter = 'blur(3px)'
      })
    },

    result(kw){
      this.typeSearch = kw
      this.search()
    },

  },
  mounted () {
    this.search()
  }
}
</script>

<style>
  html{
    min-height: 100%;
  }

  body{
    background-color: #78b0df;
    background: linear-gradient(45deg, #fff, #8abef4);
  }

  .container{
    width: 90%;
    padding: 30px;
  }

  @media only screen and (max-width: 600px) {
    .container{
      width: 100%;
      padding: 10px;
    }
  }

  li[class*=--active]{
    background-color: rgba(0,0,0,0.1);
  }

  nav.blue{
    width: 500px;
    margin-left: calc(50% - 230px);
    text-align: -webkit-center;
  }

  ul.c-sliding-pagination__list{
    width: fit-content;
  }

  .chip{
    text-transform: capitalize;
    color: #fcfcfc;
  }
</style>
