<template>
  <aside>
    <h5 v-if="arrayPokemon.length <= 0">Opa, a consulta não obteve retorno</h5>
    <div class="row">
      <div class="col s6 m4 l3 xl2" v-for="pokemon in arrayPokemon"
      v-bind:key="pokemon.id" v-on:click="detail(pokemon.id)">
        <div class="card  blue lighten-2">
          <span class="id white-text grey darken-2">#{{pokemon.id}}</span>
          <img v-bind:src="pokemon.img" v-bind:id="'pk_'+pokemon.id" v-on:error="changeIMG(pokemon.id)" alt="" class="icon">
          <span class="title">{{pokemon.name}}</span>
        </div>
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  name: 'List',
  props: {
    arrayPokemon: {
      type: Array
    },
  },
  methods: {
    detail(ev){
      this.$emit('select', ev)
    },
    changeIMG(id){
      let el = document.querySelector(`#pk_${id}`),
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
    }
  }
}
</script>

<style>
.title{
  text-transform: capitalize;
  position: relative;
  display: block;
  top: 100px;
  color: #fff;
  text-align: center;
  font-family: monospace;
  font-weight: bold;
}

span.id {
    padding: 2px 10px;
    position: relative;
    border-radius: 15px 0px 0px 0px;
    font-family: Robot;
    font-weight: bold;
    top: -1.4pt;
}

.icon{
  height: 100px;
  width: 100px;
  position: absolute;
  overflow: hidden;
  left: 30px;
  top: 20px;
  display: inline-block;
  vertical-align: middle;
  -webkit-filter: drop-shadow(12px 12px 25px rgba(0,0,0,0.5));
  filter: drop-shadow(12px 12px 25px rgba(0,0,0,0.5));
}

.card {
  border-radius: 15px;
  height: 150px;
  cursor: pointer;
}
</style>
