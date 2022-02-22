<template>
  <view class="home">
    <!-- 自定义组件---导航栏 -->
    <navbar></navbar>
    <tab :list="tabList" :tabIndex="tabIndex" @tab="tab"></tab>
    <view class="home-list">
      <list :tab="tabList" :activeIndex="activeIndex" @change="change"></list>
    </view>

  </view>
</template>

<script>
  // easyCom components/组件名/组件名.vue 局部引入
  export default {
    data() {
      return {
        tabList: [],
        tabIndex: 0,
        activeIndex: 0
      }
    },
    onLoad() {
      this.getLabel()
    },
    methods: {
      change(current) {
        this.tabIndex = current
      },
      tab(e) {
        this.activeIndex = e.index
      },
      getLabel() {
        this.$api.get_label({
          name: 'get_label'
        }).then((res) => {
          const {
            data
          } = res
          this.tabList = data
        })
      }

    }
  }
</script>

<style lang="scss">
  page {
    height: 100%;
    display: flex;
  }

  .home {
    display: flex;
    flex-direction: column;
    flex: 1;
    overflow: hidden;

    .home-list {
      flex: 1;
      box-sizing: border-box;
    }
  }
</style>
