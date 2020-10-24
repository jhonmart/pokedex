<template>
  <div class="row">
    <div class="col s12">
      <ul class="tabs tabs-fixed-width">
          <li class="tab col s3"><a class="active" href="#stat">Stats</a></li>
          <li class="tab col s3"><a href="#move">Moves</a></li>
          <li class="tab col s3"><a href="#abilitie">Abilities</a></li>
          <li class="tab col s3"><a href="#sprite">Sprites</a></li>
      </ul>
    </div>
    <div id="stat" class="col s12">
      <Stats v-bind:stats="pokemon.stats" />
    </div>
    <div id="move" class="col s12 hide">
      <Moves v-bind:moves="pokemon.moves" />
    </div>
    <div id="abilitie" class="col s12 hide">
      <Abilities v-bind:abilities="pokemon.abilities" />
    </div>
    <div id="sprite" class="col s12 hide">
      <Sprites v-bind:sprites="pokemon.sprites" />
    </div>
  </div>
</template>

<script>
import Stats from './Stats.vue'
import Moves from './Moves.vue'
import Sprites from './Sprites.vue'
import Abilities from './Abilities.vue'

export default {
  name: 'Tabs',
  components: {
    Stats,
    Moves,
    Sprites,
    Abilities
  },
  props: {
    pokemon: {
      type: Object
    }
  },
  methods: {
  },
  mounted(){
    document.querySelectorAll('.tabs a').forEach(el=>{
      el.addEventListener('click', f=>{
        let last = document.querySelector('.tabs a.active'),
            actual = document.querySelector(el.getAttribute('href'))

        last.classList.remove('active')
        el.classList.add('active')

        document.querySelector(last.getAttribute('href')).classList.add('hide')
        actual.classList.remove('hide')

        f.preventDefault()
      })
    })
  }
}
</script>

<style>
.tabs .tab a {
  color: rgb(110 121 238 / 70%);
}
.tabs .tab a:hover, .tabs .tab a.active {
  background-color: rgb(237 239 246 / 70%) !important;
  color: rgb(8 22 166 / 70%);
}
</style>