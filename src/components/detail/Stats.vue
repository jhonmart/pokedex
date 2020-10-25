<template>
  <div class="status">
    <div class="progress grey lighten-2" v-for="(s, id) in stats" v-bind:key="id" v-bind:title="'Points: ' + s.base_stat">
      <label>{{s.stat.name}}</label>
      <div v-bind:class="drawStatus(s).color" v-bind:style="drawStatus(s).width"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Stats',
  props: {
    stats: {
      type: Array
    }
  },
  methods: {
    drawStatus(stats){
      let ref = {
        'hp': 'green darken-2',
        'attack': 'red darken-1',
        'defense': 'blue darken-2',
        'special-attack': 'orange darken-1',
        'special-defense': 'purple darken-1',
        'speed': 'yellow darken-3',
      }

      return {
        width: 'width:' + stats.base_stat / 2 + '%',
        color: 'determinate ' + ref[stats.stat.name]
      }
    }
  }
}
</script>

<style>
  .progress{
    height: 20px;
    border-radius: 10px;
    padding-left: 15px;
  }

  .progress label {
    position: absolute;
    color: #f6f6f6;
    z-index: 1;
    text-transform: uppercase;
    font-weight: bold;
  }

  .status{
    margin: 20px;
  }
</style>