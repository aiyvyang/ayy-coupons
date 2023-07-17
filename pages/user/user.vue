<template>
  <view class="container">
    <view class="head">
      <view class="avatarUrl">
        <image
          class="avatar"
          src="/static/images/logo.png"
        />
      </view>
      <view class="nickName">
        优了个惠呦
      </view>
    </view>
    <view class="others">
      <view class="item">
        <image
          class="icon"
          src="/static/images/user_share.png"
        />
        <button
          class="content share"
          open-type="share"
        >
          <view>分享好友</view>
          <image
            class="right"
            src="/static/images/right_h.png"
          />
        </button>
      </view>
      <!--<view class="item">-->
      <!--  <image class="icon" src="/static/tabbar/user_fill.png"></image>-->
      <!--  <button class="content share" @tap="chouqian">-->
      <!--    <view>抽签小程序</view>-->
      <!--    <image class="right" src="/static/images/right_h.png"></image>-->
      <!--  </button>-->
      <!--</view>-->
    </view>
    <view class="version">
      v1.1.0
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
