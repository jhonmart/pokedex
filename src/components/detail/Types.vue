<template>
  <div class="types">
    <div class="chip red" v-for="(m, id) in types" v-bind:key="id">
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
}
</script>

<style>
</style>