<template>
  <list-scroll class="list-scroll" @loadMore="loadMore">
    <list-card mode="base" :item="item" v-for="item in list" :key="item.id"></list-card>
    <uni-load-more v-if="list.length===0 || list.length>9" iconType="snow" :status="load.loading"></uni-load-more>
  </list-scroll>
</template>

<script>
  export default {
    props: {
      list: {
        type: Array,
        default () {
          return []
        }
      },
      load: {
        type: Object,
        default () {
          return {
            loading: "loading"
          }
        }
      }
    },
    methods: {
      loadMore() {
        this.$emit('loadMore')
      }
    }
  }
</script>

<style>
  .list-scroll {
    height: 100%;
  }
</style>
