<template>
  <view class="l-eat-page">
    <view class="top">
      <!-- <view class="adContainer">
        <ad adpid="1617225366" @load="adLoad" @error="adError"></ad>
      </view> -->
      <view
        class="avatar"
        style="margin-top: 200px;"
      >
        <open-data type="userAvatarUrl" />
      </view>
      <view class="name">
        <open-data
          lang="zh_CN"
          type="userNickName"
        />
      </view>
    </view>
    <view class="bottom">
      <view class="eat">{{ eatLabel }}</view>
      <view class="start">
        <label
          class="but _span"
          @tap="start"
        >{{ startLabel }}</label>
      </view>
      <view
        class="custom-dish"
        @tap="openSetMean"
      >{ 自定义菜单 }</view>
    </view>
    <l-eat
      ref="setMean"
      @ok="setOk"
    />
  </view>
</template>

<script>
import LEat from "../../components/LEat/LEat.vue";

export default {
  name: "Eat",

  components: {
    LEat
  },

  data() {
    return {
      startLabel: "开始",
      eatLabel: "今天吃什么?",
      cloudDishContent: "",
      defaultDishString: "凉皮 卤粉 台湾卤肉饭 咖喱饭 嫩牛五方 寿司 新疆炒米粉 日式拉面 日式烧肉饭 杀猪粉 桥头排骨 水煮肉片 水饺 汉堡薯条 油炸烧烤 浏阳蒸菜 海南椰子鸡 潮汕砂锅粥 火锅 火锅鸡 炒粉 炒饭 烤冷面 烤肉拌饭 烧腊饭 煎饺 煎饼果子 牛丼饭 牛肉粉 牛肉面 猪肚鸡 猪脚饭 盖码饭 肉夹馍 肉汁拌饭 肠粉 臭豆腐 葱油拌面 蒸饺 蛋包饭 螺蛳粉 辣椒炒肉 过桥米线 酸菜鱼 重庆小面 锅盔 韩式炸鸡 韩式烤肉 韩式部队火锅 馄饨 鸭血粉丝 麻婆豆腐 麻辣烫 麻辣香锅 黄焖鸡 酸辣土豆丝 可乐鸡翅 麻婆豆腐 红烧肉 糖醋排骨 西红柿炒鸡蛋 青椒炒肉丝 鱼香肉丝 水煮肉片 西红柿炖牛腩 菠萝咕噜肉 咖喱鸡翅 香辣牛肉 水煮鱼 酸菜鱼 虾皮鸡蛋羹 皮蛋拌豆腐 清蒸大闸蟹 微波番茄虾 麻辣鱼 宫保鸡丁 家常豆腐 皮蛋瘦肉粥 红烧冬瓜 湘式小炒五花肉 剁椒鱼头 红烧鱼块 清蒸鲈鱼 酸辣汤 秘制红焖羊肉 酱牛肉 蒜苔炒腊肉 杭椒牛柳 红烧排骨 泡菜炒年糕 西红柿鸡蛋 土豆炖牛肉 回锅肉 木须肉 水晶猪皮冻 莲藕炖排骨 鱼香茄子 地三鲜 京酱肉丝 苦瓜炒蛋 啤酒鸭 西红柿鸡蛋汤 红烧茄子 干煸豆角 糖醋鱼 冬瓜排骨汤 凉拌黄瓜 鱼头豆腐汤 银耳莲子汤 清蒸鱼 葱油饼 米汤鸡蛋羹 蚂蚁上树 干煸菜花 香辣干锅土豆片 黄豆芽炒粉条 卤牛肉 苦瓜鸡蛋 盐烤花生米 香辣虾 小鸡炖蘑菇 蛋黄小米粥 杏仁瓦片 蜜汁叉烧排骨 栗子红烧肉 酸豆角炒鸡胗 干煸四季豆 尖椒土豆丝 青椒土豆丝 蒜香排骨 香辣烤凤爪 青椒炒肉 酸萝卜老鸭汤 葱爆羊肉 虫草花豆腐汤 酸辣蕨根粉 黄豆炖猪蹄 家常葱油饼 辣子鸡 盐水毛豆 牙签肉 避风塘炒蟹 五香毛豆 腊味煲仔饭 黑芝麻糊 炸藕盒 糯米枣 肉末香菇豆腐 洋葱拌木耳 黄金玉米烙 黄瓜拌粉皮 白灼虾 梅干菜干煸豆角 小炒圆白菜 家常红烧牛肉 炸酱面 肉末酸豆角 鲜橙蒸蛋 辣椒油 南瓜蒸百合 酸辣黄瓜 家常烧茄子"
    };
  },

  computed: {
    dishs() {
      return this.cloudDishContent.trim().split(/\s+/);
    }
  },

  onLoad() {},

  onShow() {
    this.cloudDishContent = uni.getStorageSync("meanContent") || this.defaultDishString;
  },

  methods: {
    setOk(content) {
      uni.setStorageSync("meanContent", content);
      this.cloudDishContent = content;
    },
    openSetMean() {
      this.$refs.setMean.toggleMask(this.cloudDishContent);
    },
    start() {
      this.startLabel == "开始"
        ? (this.startLabel = "停止", this.timer = setInterval(this.randEatLabel, 100))
        : (this.startLabel = "开始", clearInterval(this.timer), this.timer = null, this.eatLabel = "今天吃" + this.dish + "吧!");
    },
    randEatLabel() {
      const t = this.dishs[Math.floor(Math.random() * this.dishs.length)];
      this.eatLabel = "今天吃" + t, this.dish = t;
    },
    adLoad() {
      console.log("adLoad");
    },
    adError(e) {
      console.log("adError", e);
    }
  }
};
</script>

<style>
@import url("../../static/styles/eat.css");
</style>
