<template>
  <div class="modal-complete">
    <div id="modal-detail" class="modal transparent">
      <div class="sup">
        <i class="close material-icons red-text medium right" v-on:click="close">close</i>
      </div>

      <div class="sub">
        <div class="row">
          <img v-bind:src="pokemon.img"  class="sprite col s6 offset-s3" v-bind:id="'pk_modal_'+pokemon.id" v-on:error="changeIMG(pokemon.id)" alt="">
          
          <div class="col s12">
            <h3 class="center name-pokemon">{{pokemon.name}}</h3>
            <span class="info chip green white-text left">
              #{{pokemon.id}}
            </span>
            <Types v-bind:types="pokemon.types" />
          </div>
        </div>

        <Tabs v-bind:pokemon="pokemon" />
      </div>
    </div>

    <div class="modal-overlay" v-on:click="close"></div>
  </div>
</template>
 
<script>
import Tabs from './detail/Tabs.vue'
import Types from './detail/Types.vue'

export default {
  name: 'ModalDetails',
  components: {
    Tabs,
    Types
  },
  props: {
    pokemon: {
      name: '',
      abilities: [{ability: {name: ""}}],
      id: 0,
      stats: [],
      types: [],
      moves: []
    }
  },
  methods: {
    close(){
      document.querySelector('#modal-detail').style.display = 'none'
      document.querySelector('.modal-overlay').style.display = 'none'
      document.querySelector('#list').style.filter = 'blur(0px)'
    },
    
    changeIMG(id){
      let el = document.querySelector(`#pk_modal_${id}`),
        newIMG = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`,
        new404 = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/404.svg`

      if(el.src != newIMG && el.src != new404){
        el.parentElement.title = 'Sprite SVG not found, load PNG'
        el.alt = 'SVG not found image'
        el.src = newIMG
      } else if(el.src == newIMG){
        el.parentElement.title = 'Sprite not found "Pokemon 404"'
        el.alt = 'Not found image'
        el.src = new404
      }
    },

  }
}
</script>

<style>
  #modal-detail{
    display: none;
    width: 100%;
    position: fixed;
    top: 0;
    background: #fcfcfc;
    top: 0;
    width: 100%;
    height: 100%;
    max-height: 100%;
    z-index: 1;
  }

  .name-pokemon{
    text-transform: capitalize;
    position: relative;
    font-family: cursive;
    padding: 10px 15px;
  }

  .close{
    cursor: pointer;
    z-index: 1;
  }

  .sup{
    height: calc(50% - 280px);
  }
  
  @media only screen and (max-width: 600px) {
    .sup{
      height: calc(50% - 190px);
    }
  }


  .sub{
    border-radius: 75px 75px 0px 0px;
    background-color: #f5f5f5;
    z-index: 1;
    bottom: -20px;
    position: relative;
    min-height: calc(70% + 75px);
  }

  .name-pokemon{
    position: relative;
    z-index: 1;
  }
  
  .sprite{
    max-height: 300px;
  }

  @media only screen and (min-width: 620px) {
    .sprite[src*=png]{
      padding: 0px 250px 0px 250px;
    }
  }

  .sub .row{
    top: -120px;
    position: relative;
  }

  .modal-overlay{
    z-index: 0;
    background: #0000009e;
  }
</style>