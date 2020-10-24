<template>
  <div class="moves collection">
    <a href="#!" class="collection-item" v-for="(m, id) in moves" v-bind:key="id">
      {{m.move.name}}
    </a>
  </div>
</template>

<script>
export default {
  name: 'Moves',
  props: {
    moves: {
      type: Array
    }
  },
}
</script>

<style>
.moves{
  height: 300px;
  overflow-y: auto;
}

.moves .collection-item{
  text-transform: capitalize;;
}
</style>