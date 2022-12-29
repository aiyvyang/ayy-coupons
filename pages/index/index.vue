<template>
  <view class="container">
    <l-tabs
      v-model="current"
      :tabs="lists"
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
const result = require("../../api/coupons.json");

export default {
  name: "Index",

  components: { LTabs },

  data() {
    return {
      current: 0,
      couponList: [],
      lists: []
    };
  },

  onLoad(e) {
    this.getCoupons();

    let idx;
    // #ifdef H5
    idx = this.$route.query.idx ? parseInt(this.$route.query.idx) : 0;
    // #endif
    // #ifdef MP-WEIXIN
    idx = e.idx ? parseInt(e.idx) : 0;
    // #endif
    this.current = parseInt(idx);
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
    changeTab(i) {
      this.couponList = [];
      uni.showLoading({
        title: "获取优惠中"
      });
      this.couponList = this.lists[i] && this.lists[i].coupons || [];

      // #ifdef H5
      this.$nextTick(() => {
        this.$refs.coupon.scrollTop = 0;
      });
      // #endif
      setTimeout(() => {
        uni.hideLoading();
      }, 200);
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
      const setData = (data) => {
        this.lists = data;
        this.changeTab(0);
      };
      const env = process.env.NODE_ENV;
      if (env === "development") {
        setData(result.lists);
      } else if (env === "production") {
        uni.request({
          // eslint-disable-next-line no-undef
          url: getApp().globalData.api.coupons,
          success: (res) => {
            const { statusCode, data } = res;
            if (statusCode === 200) {
              setData(data.lists);
            }
          }
        });
      }
    }
  }
};
</script>

<style>
@import url("../../static/styles/index.css");
</style>
