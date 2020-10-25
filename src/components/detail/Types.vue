<template>
  <div class="types">
    <div class="chip" v-for="(m, id) in types" v-bind:key="id" v-bind:style="personColor(m.type.name)">
      {{m.type.name}}
    </div>
  </div>
</template>

<script>
export default {
  name: 'Types',
  props: {
    types: {
      type: Array
    }
  },
  data(){
    return {colors: {"normal": ['grey', 'grey']
                    ,"fighting": ['#bcbce0', 'grey']
                    ,"flying": ['#e0d7c3', '#4a8eae']
                    ,"poison": ['#b75b25', '#e8ba65']
                    ,"ground": ['brown', '#602401']
                    ,"rock": ['grey', '#4a1111']
                    ,"bug": ['#8a2be2', '#561515']
                    ,"ghost": ['#756c6c', 'aliceblue', '#000']
                    ,"steel": ['cadetblue', 'cornflowerblue']
                    ,"fire": ['red', 'orange']
                    ,"water": ['#00adff', '#3352a8']
                    ,"grass": ['#108310', '#74b313']
                    ,"electric": ['#ff8d00', '#cbcb32']
                    ,"psychic": ['mediumpurple', 'violet']
                    ,"ice": ['cornflowerblue', 'wheat']
                    ,"dragon": ['red', 'darkred']
                    ,"dark": ['black', 'gray']
                    ,"fairy": ['pink', '#a62037']
                    ,"unknown": ['darkolivegreen', 'darkkhaki']
                    ,"shadow": ['gray', 'darkgray']}
          }
  },
  methods: {
    personColor(ref){
      let pad = this.colors[ref]
      return `background: linear-gradient(${pad[0]}, ${pad[1]})${pad[2]? '; color: '+pad[2] : ''}`
    }
  }
}
</script>

<style>
</style>