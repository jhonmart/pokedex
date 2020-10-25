<template>
  <div class="row">
    <div class="typeSearch col s4 m2">
      <select class="browser-default col s12">
        <option value="0" selected>All</option>
        <option value="1">Name</option>
        <option value="2">Ability</option>
        <option value="3">Type</option>
        <option value="4">ID</option>
      </select>
    </div>
    <input type="text" v-on:keyup="pressKey" v-model="keyword" class="col s5 m9">
    <div class="col s3 m1 button">
      <a class="btn blue darken-1 col s12" v-on:click="search">
        <i class="material-icons">send</i>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data(){
    return {
      keyword: '',
      searchURL: '',
      types: [],
      abilitys: [],
    }
  },
  methods: {
    search(){
      this.$emit('search', this.searchURL)
    },

    pressKey(ev){
      let keyword = this.keyword.trim().toLowerCase().replace(/ /g, '-'),
          typeEl = document.querySelector('.typeSearch select')
      
      if(keyword.length){
        if(ev.key === "Enter" || ev.keyCode === 13) this.search()
        else{
          if(isNaN(+keyword)){
            let type = this.types.filter(ty=>ty.name === keyword),
                ability = this.abilitys.filter(ab=>ab.name === keyword)
                
            typeEl.value = ability.length? 2 : type.length? 3 : 1
            this.searchURL = type.length? type[0].url :
                          ability.length? ability[0].url :
                          'https://pokeapi.co/api/v2/pokemon/' + keyword
          } else {
            typeEl.value = 4
            this.searchURL = 'https://pokeapi.co/api/v2/pokemon/' + this.keyword.trim()
          }
        }
      } else{
        this.searchURL = 'https://pokeapi.co/api/v2/pokemon/'
        typeEl.value = 0

        if(ev.key === "Enter" || ev.keyCode === 13) this.search()
      }
    },
  },
  mounted () {
    fetch(`https://pokeapi.co/api/v2/type/`)
    .then(el=>el.json())
    .then(el=>{
      this.types = el.results
    })
    
    fetch(`https://pokeapi.co/api/v2/ability/?limit=300`)
    .then(el=>el.json())
    .then(el=>{
      this.abilitys = el.results
    })
  }
}
</script>

<style>
  .btn{
    height: 46px;
    padding-top: 5px !important;
  }

  .button{
    margin-left: 5px;
  }

  .typeSearch{
    padding-right: 5px;
  }
</style>