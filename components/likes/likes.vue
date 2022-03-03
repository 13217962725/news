<template>
  <view class="icons" @click.stop="likeTap">
    <uni-icons size="20" color="#f07373" :type="like?'heart-filled':'heart'"></uni-icons>
  </view>
</template>

<script>
  export default {
    name: "likes",
    props: {
      item: {
        type: Object,
        default () {
          return {}
        }
      }
    },
    data() {
      return {
        like: false
      };
    },
    watch: {
      item(newVal) {
        this.like = this.item.is_like
      }
    },
    created() {
      this.like = this.item.is_like
    },
    methods: {
      likeTap() {
        this.like = !this.like
        this.setUpdateLikes()
        console.log('ok')
      },
      setUpdateLikes() {
        uni.showLoading()
        this.$api.update_like({
          user_id: '6212f554432b16000117359d',
          article_id: this.item._id
        }).then(res => {
          uni.hideLoading()
          uni.showToast({
            title: this.like ? '收藏成功' : '取消收藏',
            icon: 'none'
          })
        }).catch(() => {
          uni.hideLoading()
        })
      }
    }
  }
</script>

<style>
  .icons {
    position: absolute;
    top: 0;
    right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 20xp;
    height: 20px;
  }
</style>
