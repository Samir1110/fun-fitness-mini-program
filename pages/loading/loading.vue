<template>
	<view>
		<view class="img-a">
			<view class="t-b">
				您好，
				<br />
				欢迎使用趣健身
			</view>
		</view>
		<view class="login-view">
			<view class="t-login">
				<form class="cl">
					<view class="t-a">
						<text class="txt">账号</text>
						<input type="text" name="ID" placeholder="请输入您的账号或手机号" maxlength="11" v-model="ID" />
					</view>
					<view class="t-a">
						<text class="txt">密码</text>
						<input type="password" name="code" maxlength="18" placeholder="请输入您的密码" v-model="pwd" />
					</view>
					<button @tap="login()">登 录</button>
					<view class="reg" @tap="reg()">注 册</view>
				</form>
				<view class="t-f"><text>—————— 第三方账号登录 ——————</text></view>
				<view class="t-e cl">
					<view class="t-g" @tap="wxLogin()"><image src="@/static/wx.png"></image></view>
					<view class="t-g" @tap="zfbLogin()"><image src="@/static/qq.png"></image></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			ID: '', 
			pwd: ''
		};
	},
	onLoad() {},
	methods: {
		login() {
			var that = this;
			const idOrPhone = that.ID.trim();
			if (!idOrPhone) {
				uni.showToast({ title: '请输入您的账号或手机号', icon: 'none' });
				return;
			}
			if (!/^\d{9}$/.test(idOrPhone) && !/^[1][3,4,5,7,8,9][0-9]{9}$/.test(idOrPhone)) {
				uni.showToast({ title: '请输入正确的账号或手机号', icon: 'none' });
				return;
			}
			if (!that.pwd) {
				uni.showToast({ title: '请输入您的密码', icon: 'none' });
				return;
			}
			uni.request({
			    url: 'http://localhost:8080/userLogin',
			    method: 'POST',
			    data: {
			        memberAccount: this.ID,
			        memberPassword: this.pwd
			    },
			    header: {
			        'Content-Type': 'application/json'
			    },
			    success: (res) => {
			        if (res.data.success) {
			            uni.setStorageSync('memberAccount', this.ID); // 存储 memberAccount
			            uni.showToast({ title: '登录成功！', icon: 'none' });
			            setTimeout(() => {
			                uni.switchTab({
			                    url: '/pages/index/index'
			                });
			            }, 1000); // 延时1秒后跳转到首页
			        } else {
			            uni.showToast({ title: res.data.message, icon: 'none' });
			        }
			    },
			    fail: () => {
			        uni.showToast({ title: '请求失败，请稍后再试', icon: 'none' });
			    }
			});



		},
		reg() {
			uni.showToast({ title: '注册跳转', icon: 'none' });
			uni.navigateTo({
				url:'./register/register',
			})
		},
		wxLogin() {
			uni.showToast({ title: '微信登录', icon: 'none' });
		},
		zfbLogin() {
			uni.showToast({ title: 'QQ登录', icon: 'none' });
		}
	}
};
</script>

<style>
.txt {
	font-size: 32rpx;
	font-weight: bold;
	color: #333333;
}
.img-a {
	width: 100%;
	height: 450rpx;
	background-image: url(../../static/head.png);
	background-size: 100%;
}
.reg {
	font-size: 28rpx;
	color: #fff;
	height: 90rpx;
	line-height: 90rpx;
	border-radius: 50rpx;
	font-weight: bold;
	background: #f5f6fa;
	color: #000000;
	text-align: center;
	margin-top: 30rpx;
}

.login-view {
	width: 100%;
	position: relative;
	margin-top: -120rpx;
	background-color: #ffffff;
	border-radius: 8% 8% 0% 0;
}

.t-login {
	width: 600rpx;
	margin: 0 auto;
	font-size: 28rpx;
	padding-top: 80rpx;
}

.t-login button {
	font-size: 28rpx;
	background: #2796f2;
	color: #fff;
	height: 90rpx;
	line-height: 90rpx;
	border-radius: 50rpx;
	font-weight: bold;
}

.t-login input {
	height: 90rpx;
	line-height: 90rpx;
	margin-bottom: 50rpx;
	border-bottom: 1px solid #e9e9e9;
	font-size: 28rpx;
}

.t-login .t-a {
	position: relative;
}

.t-b {
	text-align: left;
	font-size: 42rpx;
	color: #ffffff;
	padding: 130rpx 0 0 70rpx;
	font-weight: bold;
	line-height: 70rpx;
}

.t-login .t-c {
	position: absolute;
	right: 22rpx;
	top: 22rpx;
	background: #5677fc;
	color: #fff;
	font-size: 24rpx;
	border-radius: 50rpx;
	height: 50rpx;
	line-height: 50rpx;
	padding: 0 25rpx;
}

.t-login .t-d {
	text-align: center;
	color: #999;
	margin: 80rpx 0;
}

.t-login .t-e {
	text-align: center;
	width: 250rpx;
	margin: 80rpx auto 0;
}

.t-login .t-g {
	float: left;
	width: 50%;
}

.t-login .t-e image {
	width: 50rpx;
	height: 50rpx;
}

.t-login .t-f {
	text-align: center;
	margin: 150rpx 0 0 0;
	color: #666;
}

.t-login .t-f text {
	margin-left: 20rpx;
	color: #aaaaaa;
	font-size: 27rpx;
}

.t-login .uni-input-placeholder {
	color: #aeaeae;
}

.cl {
	zoom: 1;
}

.cl:after {
	clear: both;
	display: block;
	visibility: hidden;
	height: 0;
	content: '\20';
}
</style>
