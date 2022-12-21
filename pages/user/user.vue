<template>
  <view class="container">
    <view class="head">
      <view class="avatarUrl">
        <open-data type="userAvatarUrl" />
      </view>
      <view class="nickName">
        <open-data
          lang="zh_CN"
          type="userNickName"
        />
      </view>
    </view>
    <view class="others">
      <view class="item">
        <image
          class="icon"
          src="/static/user_share.png"
        />
        <button
          class="content share"
          open-type="share"
        >
          <view>分享好友</view>
          <image
            class="right"
            src="/static/right_h.png"
          />
        </button>
      </view>
      <!--<view class="item">-->
      <!--  <image class="icon" src="/static/user_hezuo.png"></image>-->
      <!--  <button class="content share" @tap="chouqian">-->
      <!--    <view>抽签小程序</view>-->
      <!--    <image class="right" src="/static/right_h.png"></image>-->
      <!--  </button>-->
      <!--</view>-->
    </view>
    <view class="version">
      v1.0.0
    </view>
  </view>
</template>

<script>
export default {
  name: "PagesUser",

  data() {
    return {};
  },

  onLoad() {},

  onShareAppMessage(res) {
    // eslint-disable-next-line no-undef
    return getApp().shareConfig();
  },

  methods: {
    chouqian() {
      uni.navigateToMiniProgram({
        appId: "wx84941c50415e1255",
        success(res) {
          // 打开成功
        }
      });
    }
  }
};
</script>

<style>
@import url("../../static/styles/user.css");
</style>
