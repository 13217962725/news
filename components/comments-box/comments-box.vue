<template>
  <view class="comments-box">
    <view class="comments-header">
      <view class="comments-header_logo">
        <image :src="comments.author.avatar" mode="aspectFill"></image>
      </view>
      <view class="comments-header_info">
        <view v-if="!comments.is_reply" class="title">{{comments.author.author_name}}</view>
        <view v-else class="title">{{comments.author.author_name}}<text class="reply-text">回复</text>{{comments.to}}
        </view>
        <view>{{comments.create_time | formatTime}}</view>
      </view>
    </view>
    <view class="comments-content">
      <view>{{comments.comment_content}}</view>
      <view class="comments-info">
        <view class="comments-button" @click="commentsReplay({comments,is_reply:reply})">回复</view>
      </view>
      <view class="comments-replay" v-for="item in comments.replays" :key="item.comment_id">
        <comments-box :reply="true" :comments="item" @replay="commentsReplay"></comments-box>
      </view>
    </view>
  </view>
</template>

<script>
  import commentsBox from '../../components/comments-box/comments-box.vue'
  import {
    parseTime
  } from '@/utils/index.js'
  export default {
    name: "comments-box",
    components: {
      commentsBox
    },
    props: {
      comments: {
        type: Object,
        default () {
          return {}
        }
      },
      reply: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {

      };
    },
    filters: {
      formatTime(time) {
        return parseTime(time)
      }
    },
    methods: {
      commentsReplay(comment) {
        // 为了区分 主回复,还是子回复
        if (comment.is_reply) {
          comment.comments.reply_id = comment.comments.comment_id
          comment.comments.comment_id = this.comments.comment_id
        }
        this.$emit('replay', comment)
      }
    }
  }
</script>

<style lang="scss">
  .comments-box {
    margin: 15px 0;

    .comments-header {
      display: flex;

      .comments-header_logo {
        flex-shrink: 0;
        width: 30px;
        height: 30px;
        border-radius: 5px;
        overflow: hidden;

        image {
          width: 100%;
          height: 100%;
        }
      }

      .comments-header_info {
        display: flex;
        flex-direction: column;
        padding-left: 15px;
        font-size: 12px;
        color: #999999;
        line-height: 1;

        .title {
          margin-bottom: 10px;
          font-size: 14px;
          color: #333333;

          .reply-text {
            margin: 0 10px;
            font-weight: bold;
            color: #000000;
          }
        }
      }
    }

    .comments-content {
      margin-top: 10px;
      font-size: 14px;
      color: #000;

      .comments-info {
        margin-top: 15px;
        display: flex;

        .comments-button {
          padding: 2px 10px;
          font-size: 12px;
          color: #999999;
          border-radius: 10px;
          border: 1px solid #ccc;
        }
      }

      .comments-replay {
        display: flex;
        margin: 15px 0;
        padding: 0 10px;
        border: 1px solid #eee;
      }
    }
  }
</style>
