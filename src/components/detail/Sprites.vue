<template>
  <div class="sprites">

    <div  v-for="(m, id) in sprites" v-bind:key="id">
      <div class="card" v-if="m && typeof m == 'string'">
        <div class="card-image">
          <img v-bind:src="m">
        </div>
        <span class="card-title black-text">{{id}}</span>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Sprites',
  props: {
    sprites: {
      type: Object
    }
  },
}
</script>

<style>
  .sprites .card{
    color: black;
    float: left;
    margin-right: 10px;
    padding: 5px;
    text-align: -webkit-center;
  }

  .sprites .card-image{
    width: 100px;
    height: 100px;
  }

  .card-image .card-title{
    bottom: 2px !important;
    position: relative !important
  }
</style>