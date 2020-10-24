<template>
  <div class="status">
    <div class="progress grey lighten-2" v-for="(s, id) in stats" v-bind:key="id">
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
        'hp': 'red darken-1',
        'attack': 'purple darken-1',
        'defense': 'pink darken-2',
        'special-attack': 'yellow darken-3',
        'special-defense': 'green darken-2',
        'speed': 'orange darken-1',
      }

      return {
        width: 'width:' + stats.base_stat + '%',
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