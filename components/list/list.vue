<template>
  <swiper class="home-swiper" :current="activeIndex" @change="change">
    <swiper-item v-for="(item,index) in tab" :key="index" class="swiper-item">
      <listItem></listItem>
    </swiper-item>
  </swiper>
</template>

<script>
  import listItem from './list-item.vue'
  export default {
    name: "list",
    props: {
      tab: {
        type: Array,
        default () {
          return []
        }
      },
      activeIndex: {
        type: Number,
        default: 0
      }
    },
    components: {
      listItem
    },
    data() {
      return {

      };
    },
    methods: {
      change(e) {
        console.log(e)
        const {
          current
        } = e.detail
        this.$emit('change', current)
      }
    }
  }
</script>

<style lang="scss">
  .home-swiper {
    height: 100%;

    .swiper-item {
      height: 100%;
      overflow: hidden;
    }
  }
</style>
