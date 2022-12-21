<template>
  <view class="container">
    <l-tabs
      v-model="current"
      :tabs="tabs"
      class="tab"
      @change="changeTab"
    />
    <view
      ref="coupon"
      class="coupon"
    >
      <view
        v-for="(v, i) in couponList"
        :key="i"
        class="item"
        @click="toCoupon(i)"
      >
        <view class="top">
          <view class="left">
            <view class="content">
              <image
                :src="v.icon"
                class="icon"
                mode="widthFix"
              />
              <view class="name">{{ v.name }}</view>
            </view>
            <view
              v-if="v.type == 1"
              class="text"
            >天天可领</view>
            <view
              v-else-if="v.type == 2"
              class="text"
            >限时秒杀</view>
          </view>
          <view class="right">免费领取</view>
        </view>
        <view class="bottom">
          <image
            :src="v.bannerPic"
            mode="widthFix"
          />
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import LTabs from "../../components/LTabs/LTabs.vue";

export default {
  name: "Index",

  components: { LTabs },

  data() {
    return {
      current: 0,
      tabs: [],
      couponList: [],
      coupons: []
    };
  },

  onLoad(e) {
    this.getCoupons();

    let tabId;
    // #ifdef H5
    tabId = this.$route.query.tabId ? parseInt(this.$route.query.tabId) : 0;
    // #endif
    // #ifdef MP-WEIXIN
    tabId = e.tabId ? parseInt(e.tabId) : 0;
    // #endif
    for (const i in this.tabs) {
      if (tabId == this.tabs[i].tabId) {
        this.current = parseInt(i);
      }
    }
    this.changeTab(this.current);
  },

  onShareAppMessage(res) {
    // eslint-disable-next-line no-undef
    return getApp().shareConfig();
  },

  onShareTimeline(res) {
    // eslint-disable-next-line no-undef
    return getApp().shareConfig();
  },

  methods: {
    changeTab(index) {
      this.couponList = [];
      uni.showLoading({
        title: "获取优惠中"
      });
      if (index == 0) {
        this.couponList = this.coupons;
      } else {
        for (const i in this.coupons) {
          if (this.coupons[i].tabId == this.tabs[index].tabId) {
            this.couponList.push(this.coupons[i]);
          }
        }
      }
      // #ifdef H5
      this.$nextTick(() => {
        this.$refs.coupon.scrollTop = 0;
      });
      // #endif
      setTimeout(() => {
        uni.hideLoading();
      }, 500);
    },
    toCoupon(i) {
      const currentOffer = this.couponList[i];

      // #ifdef H5
      window.location.href = currentOffer.url;
      // #endif
      // #ifdef MP-WEIXIN
      if (currentOffer.minapp) {
        uni.navigateToMiniProgram({
          appId: currentOffer.minapp.appid,
          path: currentOffer.minapp.path,
          success(res) {
            // 打开成功
          }
        });
      }
      // #endif
    },
    getCoupons() {
      uni.request({
        // eslint-disable-next-line no-undef
        url: getApp().globalData.api.coupons,
        success: (res) => {
          const { statusCode, data } = res;
          if (statusCode === 200) {
            this.tabs = data.tabs;
            this.coupons = data.coupons;
            this.changeTab(0);
          }
        }
      });
    }
  }
};
</script>

<style>
@import url("../../static/styles/index.css");
</style>
