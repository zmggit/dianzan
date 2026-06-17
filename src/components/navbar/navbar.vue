<template>
  <view class="navbar" :style="{ backgroundColor: bgColor, color: textColor }">
    <!-- 模拟系统状态栏 (刘海/灵动岛区域) -->
    <view class="status-bar">
      <text class="time">{{ pageData.navbarTime || '18:05' }}</text>
      
      <view class="status-right">
        <!-- 信号图标 (建议替换为更贴近iOS的字体图标) -->
        <text class="iconfont icon_xinhao">&#xe61f;</text>
        <text class="nav_5g" :style="{ color: textColor }">5G</text>
        <text class="iconfont icon_wifi">&#xe627;</text>
        
        <!-- 优化后的电池图标 -->
        <view class="battery-box" :style="{ borderColor: textColor }">
          <!-- 电池右侧的凸起小头 -->
          <view class="battery-nub" :style="{ backgroundColor: textColor }"></view>
          <!-- 电池容量填充 -->
          <view
            class="battery-fill"
            :style="{
              width: (pageData.dian || 68) + '%',
              backgroundColor: (pageData.dian || 68) <= 20 ? '#ff3b30' : textColor
            }"
          ></view>
          <!-- 电池数字 -->
          <text 
            class="battery-num" 
            :style="{ color: bgColor }"
          >{{ pageData.dian || 68 }}</text>
        </view>
      </view>
    </view>

    <!-- 微信导航栏 -->
    <view class="nav-bar">
      <view class="nav-left" @tap="back">
        <text class="iconfont icon_left">&#xe60a;</text>
      </view>
      
      <!-- 使用绝对定位保证标题永远居中 -->
      <view class="nav-title">
        <text>{{ ismain ? pageData.article.date.date + " " + pageData.article.date.time : "详情" }}</text>
      </view>
      
      <view class="nav-right" @tap="handleRight">
        <text class="iconfont icon_right">&#xf0141;</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: "navbar",
  props: {
    pageData: {
      type: Object,
      default: () => ({
        navbarTime: '18:05',
        dian: 68
      }),
    },
    bgColor: {
      type: String,
      default: "#f7f7f7",
    },
    textColor: {
      type: String,
      default: "#000000", // iOS标准状态栏通常是纯黑
    },
    ismain: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    back() {
      uni.navigateBack({
        delta: 1,
      });
    },
    handleRight(e) {
      let data = {
        x: e.detail.x - 100,
        y: e.detail.y,
      };
      this.$emit("handle", data);
    },
  },
};
</script>

<style lang="scss" scoped>
.navbar {
  width: 100%;
  box-sizing: border-box;
  /* 增加顶部 padding 模拟 iOS 灵动岛/刘海的安全距离 */
  padding-top: 80rpx; 
  position: relative;
  z-index: 99;

  /* --- 模拟系统状态栏 --- */
  .status-bar {
    width: 100%;
    height: 44rpx;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 40rpx;
    margin-bottom: 20rpx;

    .time {
      font-size: 32rpx;
      font-weight: 600;
      letter-spacing: 1rpx;
      font-family: -apple-system, Helvetica, sans-serif; /* 尽量逼近苹方/SF字体 */
    }

    .status-right {
      display: flex;
      align-items: center;
      gap: 12rpx; /* 图标之间的间距 */

      .icon_xinhao {
        font-size: 26rpx;
      }
      
      .nav_5g {
        font-size: 22rpx;
        font-weight: 600;
      }

      .icon_wifi {
        font-size: 32rpx;
      }

      /* 极致还原的 iOS 电池样式 */
      .battery-box {
        width: 52rpx;
        height: 26rpx;
        box-sizing: border-box;
        border: 2rpx solid;
        border-radius: 8rpx; /* 更圆润 */
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-left: 4rpx;

        .battery-nub {
          position: absolute;
          right: -6rpx;
          top: 50%;
          transform: translateY(-50%);
          width: 4rpx;
          height: 10rpx;
          border-radius: 0 3rpx 3rpx 0;
          opacity: 0.6;
        }

        .battery-fill {
          position: absolute;
          left: 2rpx;
          top: 2rpx;
          bottom: 2rpx;
          border-radius: 4rpx;
          transition: width 0.3s ease;
        }

        .battery-num {
          position: relative;
          z-index: 2;
          font-size: 20rpx;
          font-weight: 700;
          transform: scale(0.9); /* 缩小数字使其完美塞入电池 */
          /* iOS 真实效果是数字把背景掏空，这里为了兼容性使用与背景同色的文字盖在上面 */
        }
      }
    }
  }

  /* --- 微信导航栏 --- */
  .nav-bar {
    width: 100%;
    height: 88rpx; /* 标准导航栏高度 */
    position: relative; /* 为绝对居中的标题做准备 */
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 30rpx;
    box-sizing: border-box;

    .nav-left {
      width: 80rpx;
      height: 100%;
      display: flex;
      align-items: center;
      .icon_left {
        font-size: 44rpx;
        font-weight: 500;
      }
    }

    /* 标题绝对居中 */
    .nav-title {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      font-size: 34rpx;
      font-weight: 500;
      letter-spacing: 2rpx;
      max-width: 50%;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .nav-right {
      width: 80rpx;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      .icon_right {
        font-size: 40rpx;
        font-weight: 600;
      }
    }
  }
}
</style>