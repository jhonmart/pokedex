<template>
  <div class="abilities">
    <div class="chip blue" v-for="(a, id) in abilities" v-bind:key="id">
      {{a.ability.name}}
    </div>
  </div>
</template>

<script>
export default {
  name: 'Abilities',
  props: {
    abilities: {
      type: Array
    }
  },
}
</script>

<style>
.abilities{
  padding-top: 10px;
}
</style>