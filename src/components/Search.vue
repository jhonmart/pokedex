<template>
  <div class="row">
    <input type="text" v-on:keyup="pressKey" v-model="keyword" class="col s11">
    <div class="col s1 button">
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
      let keyword = this.keyword.trim().toLowerCase()
      
      if(keyword.length){
        if(ev.key === "Enter" || ev.keyCode === 13) this.search()
        else{
          if(isNaN(+keyword)){
            let type = this.types.filter(ty=>ty.name === keyword),
                ability = this.abilitys.filter(ab=>ab.name === keyword)
                
            this.searchURL = type.length? type[0].url :
                          ability.length? ability[0].url :
                          'https://pokeapi.co/api/v2/pokemon/' + keyword
          } else {
            this.searchURL = 'https://pokeapi.co/api/v2/pokemon/' + this.keyword.trim()
          }
        }
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
</style>