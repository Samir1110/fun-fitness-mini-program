<template>
  <view class="content">
    <image class="logo" src="/static/user1.png"></image>
    <view class="text-area">
      <text class="title">{{title}}</text>
    </view>
    <view class="text-area">
      <text class="date">{{date}}</text>
    </view>
    <button class="sign-in-btn" @click="signIn">签到</button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: '',  // 初始为空
      date: '',
      memberAccount: ''  // 初始为空，稍后从存储中获取
    }
  },
  onLoad() {
    // 获取存储的用户ID
    this.memberAccount = uni.getStorageSync('memberAccount');
    // 调用方法获取用户信息
    if (this.memberAccount) {
      this.fetchUserInfo();
    } else {
      console.error('用户ID未找到，请重新登录');
    }
  },
  methods: {
    async fetchUserInfo() {
      try {
        const res = await uni.request({
          url: 'http://localhost:8080/user/toUserInfo',
          method: 'GET',
          data: {
            memberAccount: this.memberAccount  // 使用从存储中获取的用户ID
          },
          header: {
            'Content-Type': 'application/json'
          }
        });
        
        if (res.statusCode === 200 && res.data && res.data.success) {
          this.title = res.data.members[0].memberName;  // 假设用户信息中有一个memberName字段
        } else {
          console.error('Response error:', res.data.message);
        }
      } catch (error) {
        console.error('Catch error:', error);
      }
    },
    signIn() {
      if (!this.memberAccount) {
        uni.showToast({
          title: '用户未登录',
          icon: 'none'
        });
        return;
      }

      uni.request({
        url: 'http://localhost:8080/gym/AddPeople', // 替换为你的后端接口地址
        method: 'POST',
        header: {
          'Content-Type': 'application/x-www-form-urlencoded' // 确保Content-Type正确
        },
        data: {
          memberAccount: this.memberAccount
        },
        success: (res) => {
          if (res.statusCode === 200) {
            uni.showToast({
              title: '签到成功',
              icon: 'success'
            });
          } else {
            uni.showToast({
              title: '签到失败',
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
}
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20rpx;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 100rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 20rpx;
}

.text-area {
  display: flex;
  justify-content: center;
  margin-bottom: 20rpx;
}

.title {
  font-size: 42rpx;
  color: #000000;
}

.date {
  font-size: 36rpx;
  color: #666666;
}

.sign-in-btn {
  width: 200rpx;
  height: 80rpx;
  background-color: #007BFF;
  color: #fff;
  border: none;
  border-radius: 10rpx;
  text-align: center;
  line-height: 80rpx;
  font-size: 36rpx;
  margin-top: 40rpx;
}
</style>
