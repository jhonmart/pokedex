<template>
  <div id="app" class="container">
    <div id="list">
      <Search v-on:search="result"/>
      <List v-bind:arrayPokemon="pokemons" v-on:select="detail"/>


      <sliding-pagination
        v-if="totalPages>1"
        class="blue darken-2"
        :current="currentPage"
        :total="totalPages"
        @page-change="pageChangeHandler"
      ></sliding-pagination>
    </div>

    <ModalDetails v-bind:pokemon="pokemonSelect"/>
  </div>
</template>

<script>
import List from './components/List.vue'
import Search from './components/Search.vue'
import ModalDetails from './components/ModalDetails.vue'
import SlidingPagination from 'vue-sliding-pagination'

export default {
  name: 'App',
  components: {
    List,
    Search,
    SlidingPagination,
    ModalDetails
  },
  data(){
    return {
      pokemons: [{
        id: 0,
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
      qtdView: 20,

      keyword: '',
      typeSearch: 'https://pokeapi.co/api/v2/pokemon/'
    }
  },
  methods: {
    search(start = this.start){
      fetch(`${this.typeSearch}?offset=${start}&limit=${this.qtdView}`)
      .then(el=>el.json())
      .then(el=>{
          let listItens = (el.results || el.pokemon)
          if(el.abilities && !listItens){
            this.pokemons = [{
              id: el.id,
              name: el.name,
              url: 'https://pokeapi.co/api/v2/pokemon/'+ el.id,
              img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${el.id}.svg`
            }]

            this.totalPages = 0
          } else{
            this.pokemons = listItens.map(camps=>({
              ...camps,
              id: (camps.url || camps.pokemon.url).split('/')[6],
              img: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${(camps.url || camps.pokemon.url).split('/')[6]}.svg`
            })) || []

            this.totalPages = Math.ceil(el.count/this.qtdView)
          }
      })
    },

    pageChangeHandler(selectedPage) {
      this.currentPage = selectedPage

      this.search(this.qtdView*(selectedPage-1))
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
        console.log(el)
      })
    },

    result(kw){
      this.typeSearch = kw
      this.search()
      console.log('Pesquisar', kw)
    },

  },
  mounted () {
    this.search()
  }
}
</script>

<style>
  body{
    background-color: #fcfcfc;
    background: linear-gradient(45deg, #fff, #fcfcfc);
  }

  .container{
    width: 90%;
    padding: 30px;
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
