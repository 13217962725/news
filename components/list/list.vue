<template>
  <swiper class="home-swiper" :current="activeIndex" @change="change">
    <swiper-item v-for="(item,index) in tab" :key="index" class="swiper-item">
      <listItem :list="listCatchData[index]" :load="load[index]" @loadMore="loadMore"></listItem>
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
        list: [],
        // js 的限制 listCatchData[index]=data
        listCatchData: {},
        load: {},
        pageSize: 10
      };
    },
    // onLoad在页面,created在组件
    created() {
      // TODO tab 还没有赋值
      uni.$on('update_article', () => {
        this.listCatchData = {}
        this.load = {}
        this.getList(this.activeIndex)
      })
    },
    watch: {
      tab(newVal) {
        if (newVal.length === 0) return
        this.listCatchData = {}
        this.load = {}
        this.getList(this.activeIndex)
      }
    },
    methods: {
      loadMore() {
        if (this.load[this.activeIndex].loading === 'noMore') return
        this.load[this.activeIndex].page++
        console.log('触发上拉')
        this.getList(this.activeIndex)
      },
      change(e) {
        const {
          current
        } = e.detail
        this.$emit('change', current)
        // TODO 当数据不存在 或者 长度是 0 的情况下才去请求数据
        // console.log(this.listCatchData[current])
        if (!this.listCatchData[current] || this.listCatchData[current].length === 0) {
          this.getList(current)
        }
      },
      getList(current) {
        if (!this.load[current]) {
          this.load[current] = {
            page: 1,
            loading: 'loading'
          }
        }
        // console.log(this.load[current].page)
        this.$api.get_list({
          name: this.tab[current].name,
          page: this.load[current].page,
          pageSize: this.pageSize
        }).then(res => {
          console.log(res)
          const {
            data
          } = res
          if (data.length === 0) {
            let oldLoad = {}
            oldLoad.loading = 'noMore',
              oldLoad.page = this.load[current].page
            this.$set(this.load, current, oldLoad)
            // 强制渲染页面
            this.$forceUpdate()
            return
          }
          // 懒加载
          let oldList = this.listCatchData[current] || []
          oldList.push(...data)
          this.$set(this.listCatchData, current, oldList)
        })
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
      touch-action: none;
    }
  }
</style>
