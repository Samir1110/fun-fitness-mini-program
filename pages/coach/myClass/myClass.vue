<template>
  <view class="container">
    <block v-for="order in classOrderList" :key="order.classOrderId">
      <view class="class-card">
        <view class="class-info">
          <text class="class-name">{{ order.className }}<br></text>
          <text class="class-coach">教练：{{ order.coach }}<br></text>
          <text class="class-time">时间：{{ order.classBegin }}</text>
        </view>
        <button class="cancel-btn" @click="cancelClass(order.classOrderId)">退课</button>
      </view>
    </block>
  </view>
</template>

<script>
export default {
  data() {
    return {
      classOrderList: []
    };
  },
  onLoad() {
    this.fetchClassOrderList();
  },
  methods: {
    fetchClassOrderList() {
      const memberAccount = uni.getStorageSync('memberAccount'); // 获取存储的用户账号
      if (!memberAccount) {
        uni.showToast({
          title: '用户未登录',
          icon: 'none'
        });
        return;
      }

      uni.request({
        url: 'http://localhost:8080/user/toUserClass', // 替换为你的后端接口地址
        method: 'GET',
        data: {
          memberAccount: memberAccount
        },
        success: (res) => {
          if (res.statusCode === 200) {
            // 对数据按classBegin排序
            this.classOrderList = res.data.sort((a, b) => this.parseDate(a.classBegin) - this.parseDate(b.classBegin));
          } else {
            uni.showToast({
              title: '获取课程信息失败',
              icon: 'none'
            });
          }
        },
        fail: () => {
          uni.showToast({
            title: '请求失败',
            icon: 'none'
          });
        }
      });
    },
    parseDate(dateString) {
      const datePattern = /(\d{4})年(\d{1,2})月(\d{1,2})日\s+(\d{1,2}):(\d{2})/;
      const match = dateString.match(datePattern);
      if (match) {
        const [_, year, month, day, hour, minute] = match;
        return new Date(year, month - 1, day, hour, minute);
      }
      return new Date();
    },
    cancelClass(classOrderId) {
      const memberAccount = uni.getStorageSync('memberAccount'); // 获取存储的用户账号
      if (!memberAccount) {
        uni.showToast({
          title: '用户未登录',
          icon: 'none'
        });
        return;
      }

      console.log(`Sending request with classOrderId: ${classOrderId}`);

      uni.request({
        url: 'http://localhost:8080/user/delUserClass', // 替换为你的后端接口地址
        method: 'POST',
        header: {
          'Content-Type': 'application/x-www-form-urlencoded' // 确保Content-Type正确
        },
        data: {
          classOrderId: classOrderId
        },
        success: (res) => {
          if (res.data.success) {
            uni.showToast({
              title: res.data.message,
              icon: 'success'
            });
            this.fetchClassOrderList(); // 重新获取课程列表
          } else {
            uni.showToast({
              title: res.data.message,
              icon: 'none'
            });
          }
        },
        fail: () => {
          uni.showToast({
            title: '请求失败',
            icon: 'none'
          });
        }
      });
    }
  }
};
</script>

<style>
.container {
  padding: 20rpx;
}

.class-card {
  background-color: #fff;
  padding: 20rpx;
  margin-bottom: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.1);
}

.class-info {
  margin-bottom: 20rpx;
}

.class-name {
  font-size: 32rpx;
  font-weight: bold;
}

.class-coach,
.class-time {
  font-size: 28rpx;
  color: #666;
  margin-top: 10rpx;
}

.cancel-btn {
  width: 100%;
  padding: 10rpx;
  background-color: #ff4d4f;
  color: #fff;
  border: none;
  border-radius: 5rpx;
  text-align: center;
}
</style>
