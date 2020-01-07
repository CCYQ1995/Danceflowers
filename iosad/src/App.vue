<template>
  <div id="app">
    <!-- Swiper -->
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div
          class="swiper-slide"
          v-if="horizontal"
        >
          <img
            src="./assets/ad1.png"
            alt=""
          />
        </div>
        <div
          class="swiper-slide"
          v-if="horizontal"
        >
          <img
            src="./assets/ad2.png"
            alt=""
          />
        </div>
        <div
          class="swiper-slide"
          v-if="!horizontal"
        >
          <img
            src="./assets/ad3.jpg"
            alt=""
          />
        </div>
        <div
          class="swiper-slide"
          v-if="!horizontal"
        >
          <img
            src="./assets/ad4.jpg"
            alt=""
          />
        </div>
      </div>
    </div>
    <div class="countdown">
      <div @click="closeAd">{{ text }}</div>
    </div>
    <div class="adTips">
      <span>广告</span>
    </div>
  </div>
</template>

<script>
import "swiper/css/swiper.min.css";
import Swiper from "swiper";
export default {
  name: "app",
  data() {
    return {
      time: 10,
      text: "",
      /**
       * 是否横竖（true 默认竖屏）
       */
      horizontal: true
    };
  },
  created() {
    // horizontal字段1就是竖屏，否则为横屏
    let horizontalValue = this.getQueryString("horizontal");
    console.log(horizontalValue);

    if (horizontalValue != "1") {
      this.horizontal = false;
    }
  },
  mounted() {
    const that = this;
    new Swiper(".swiper-container", {
      autoplay: true,
      loop: true,
      on: {
        tap: function() {
          that.openApp();
        }
      }
    });
    this.text = this.time + "s";

    let clearTime = setInterval(() => {
      this.time--;
      if (this.time <= 0) {
        clearInterval(clearTime);
        this.text = "关闭";
        return;
      }
      this.text = this.time + "s";
    }, 1000);
  },
  methods: {
    closeAd() {
      if (this.text === "关闭") {
        try {
          window.webkit.messageHandlers.closeAd.postMessage(null);
        } catch (error) {
          console.log(error);
        }
      }
    },
    /**
     *@function getQueryString 获取链接后面拼接的参数
     *@param {name} 要获取的字段名
     */
    getQueryString(name) {
      var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
      var r = window.location.search.substr(1).match(reg);
      if (r != null) {
        return unescape(r[2]);
      } else {
        return null;
      }
    },
    openApp() {
      var iOSUrl = "taobao://"; //填URL Schemes

      var isIOS = /(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent);

      //判断是否是iOS
      if (isIOS) {
        window.location.href = iOSUrl;

        // var loadDataTime = Date.now();
        // setTimeout(() => {
        //   var timeOutDateTime = Date.now();
        //   if (timeOutDateTime - loadDataTime < 1000) {
        //     window.location.href = "https://www.apple.com/ios/app-store/"; //填下载App落地页
        //   }
        // }, 25);
      }
    }
  }
};
</script>

<style>
html,
body {
  position: relative;
  height: 100%;
}
#app,
.swiper-container,
.swiper-wrapper,
.swiper-slide {
  width: 100%;
  height: 100%;
}
body {
  background: #eee;
  font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
  font-size: 14px;
  color: #000;
  margin: 0;
  padding: 0;
}
.swiper-container {
  width: 100%;
  height: 100%;
}
.swiper-slide {
  text-align: center;
  font-size: 18px;
  background: #fff;
  /* Center slide text vertically */
  display: -webkit-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  -webkit-justify-content: center;
  justify-content: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  -webkit-align-items: center;
  align-items: center;
}
.swiper-slide img {
  width: 100%;
  height: 100%;
}
.countdown {
  position: fixed;
  right: 30px;
  top: 20px;
  z-index: 999;
}
.countdown > div {
  line-height: 40px;
  text-align: center;
  color: #fff;
}
.countdown > div:first-child {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.4);
}
.adTips {
  position: fixed;
  left: 30px;
  top: 20px;
  z-index: 999;
}
.adTips > span {
  display: block;
  width: 60px;
  height: 40px;
  border-radius: 20px;
  background-color: rgba(0, 0, 0, 0.4);
  line-height: 40px;
  text-align: center;
  color: #fff;
}
</style>
