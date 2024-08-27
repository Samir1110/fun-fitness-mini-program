<!-- 蓝色登录页面2 -->
<template>
	<view>
		<view class="login-view" style="">
			<view class="t-login">
				<form class="cl">
					<view class="t-a">
						" "
					    <input type="string" name="name" placeholder="请输入您的姓名" />
					    <input type="number" name="phone" placeholder="请输入您的手机号" maxlength="11" v-model="phone" />
					    
					</view>
					<view class="t-a">
						
						<input type="password" name="code" maxlength="18" placeholder="请输入您的密码" v-model="pwd" />
						<input type="password" name="code" maxlength="18" placeholder="请确认您的密码" v-model="pwd" />
					</view>
					<view class="t-a">
					    
					    <input type="string"  placeholder="请输入健身房推荐码" maxlength="11" v-model="phone" />
					    
					</view>
					<!--<button @tap="login()">登 录</button>-->
					<view class="reg" @tap="reg()">注 册</view>
				</form>
				
				
			</view>
		</view>
	</view>
</template>
<script>
export default {
	/**
	 * 该模板只是登录模板，如果需要注册，直接复制该页面稍微改动即可
	 */
	data() {
		return {
			name: '',
			phone: '', //手机号码
			pwd: '' //密码
		};
	},
	onLoad() {},
	methods: {
		//当前登录按钮操作
		login() {
			var that = this;
			if (!that.phone) {
				uni.showToast({ title: '请输入您的手机号', icon: 'none' });
				return;
			}
			if (!/^[1][3,4,5,7,8,9][0-9]{9}$/.test(that.phone)) {
				uni.showToast({ title: '请输入正确手机号', icon: 'none' });
				return;
			}
			if (!that.pwd) {
				uni.showToast({ title: '请输入您的密码', icon: 'none' });
				return;
			}
			uni.showToast({ title: '登录成功！', icon: 'none' });
		},
		//注册按钮点击
		reg() {
			uni.showToast({ title: '注册成功', icon: 'none' });
		},
		
	}
};
</script>
<style>
.txt {
	font-size: 32rpx;
	font-weight: bold;
	color: #333333;
}
/* .img-a {
	width: 100%;
	height: 450rpx;
	background-image: url(../../static/head.png);
	background-size: 100%;
} */
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
	margin-top: -100rpx;
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


.small_button {
    font-size: 20px; /* 调整按钮字体大小 */
    padding: 5px 10px; /* 调整按钮内边距 */
	color: black;
}

</style>
