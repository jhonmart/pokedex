<template>
  <div class="row">
    <sliding-pagination
        v-if="totalPages>1"
        class="blue darken-2 col s12"
        :current="currentPage"
        :total="totalPages"
        :slidingEndingSize="size<500? 1 : 2"
        :slidingWindowSize="size<500? 1 : 3"
        @page-change="pageChangeHandler"
    ></sliding-pagination>
  </div>
</template>

<script>
import SlidingPagination from 'vue-sliding-pagination'
export default {
  name: 'Pagination',
  components:{
    SlidingPagination,
  },
  props: {
    currentPage: {
      type: Number
    },
    totalPages: {
      type: Number
    },
    pageChangeHandler: {
      type: Function
    }
  },
  data(){
    return {
      size: document.body.offsetWidth
    }
  }
}
</script>