<template>
  <view class="container">
    <block v-for="classItem in classList" :key="classItem.classId">
      <view class="class-card">
        <view class="class-info">
          <text class="class-name">{{ classItem.className }}<br></text>
          <text class="class-coach">教练：{{ classItem.coach }}<br></text>
          <text class="class-time">时间：{{ classItem.classBegin }}<br>时长：{{ classItem.classTime }}</text>
        </view>
        <button class="apply-btn" @click="applyClass(classItem.classId)">选课</button>
      </view>
    </block>
  </view>
</template>

<script>
export default {
  data() {
    return {
      classList: []
    };
  },
  onLoad() {
    this.fetchClassList();
  },
  methods: {
    fetchClassList() {
      uni.request({
        url: 'http://localhost:8080/class/selClass', // 替换为你的后端接口地址
        method: 'GET',
        success: (res) => {
          if (res.statusCode === 200) {
            // 对数据按classBegin排序
            this.classList = res.data.sort((a, b) => this.parseDate(a.classBegin) - this.parseDate(b.classBegin));
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
    applyClass(classId) {
      const memberAccount = uni.getStorageSync('memberAccount'); // 获取存储的用户账号
      if (!memberAccount) {
        uni.showToast({
          title: '用户未登录',
          icon: 'none'
        });
        return;
      }
      console.log(`Sending request with classId: ${classId} and memberAccount: ${memberAccount}`);
      uni.request({
        url: 'http://localhost:8080/user/applyClass', // 替换为你的后端接口地址
        method: 'POST',
        header: {
          'content-type': 'application/x-www-form-urlencoded' // 设置请求头为表单格式
        },
        data: {
          classId: classId,
          memberAccount: memberAccount
        },
        success: (res) => {
          if (res.data.success) {
            uni.showToast({
              title: res.data.message,
              icon: 'success'
            });
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

.apply-btn {
  width: 100%;
  padding: 10rpx;
  background-color: #007BFF;
  color: #fff;
  border: none;
  border-radius: 5rpx;
  text-align: center;
}
</style>
